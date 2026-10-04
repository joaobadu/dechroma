# Dechroma — Project Status

## Project

**Name:** Dechroma
**Status:** Early research and development
**Current phase:** Project definition and architecture
**Target platforms:** Windows, macOS, Linux
**Language:** English
**Repository:** GitHub

## Purpose

Dechroma is an open-source, cross-platform application designed to remove unwanted chromaticity from photographs and scans of documents whose legitimate content is expected to be exclusively monochrome.

The core assumption is:

> The document is monochrome. Black and white are information; unwanted chromaticity is an artifact.

The primary technical challenge is to distinguish unwanted photographic or scanning chromaticity from legitimate dark document content without unnecessarily altering the latter.

## Scope of the Initial Version

The first version should provide:

* Open an image.
* Display the original image.
* Zoom and pan.
* Generate a chromaticity mask.
* Display the mask.
* Display a diagnostic overlay.
* Display original and processed images.
* Compare images with synchronized zoom and position.
* Adjust chromaticity-related parameters.
* Define a white reference automatically.
* Define a white reference using a color picker.
* Inspect individual pixels.
* Apply chromaticity removal.
* Experiment with chromaticity neutralization.
* Export the processed image.
* Save the parameters used for processing.

## Explicitly Out of Scope

The initial version will not attempt to provide:

* OCR.
* Perspective correction.
* Deskewing.
* General image editing.
* Document restoration.
* Complex illumination correction.
* Shadow removal.
* Binarization.
* Black stain removal.
* Advanced batch processing.
* A complete document-processing workflow.

These capabilities may be considered later, but they are not part of the initial product definition.

## Core Architecture

The intended architecture is:

```text
GUI
 │
 ▼
Core Library
 │
 ▼
Image Processing Backend
 │
 ▼
Image I/O
```

The core algorithm must remain independent of the graphical interface.

The application should also be designed so that the same Core Library can later be used by:

* GUI;
* CLI;
* batch processing;
* scripts;
* other applications.

## Technology Direction

The initial implementation will use the following technology stack:

```text
Programming language:
C++

GUI framework:
Qt 6

Build system:
CMake
```

This technology stack is selected for the initial implementation based on the project's requirements for:

* cross-platform desktop operation;
* processing of large images;
* pixel-level image processing;
* responsive image visualization;
* zoom and pan;
* integration between the GUI and the Core Library;
* long-term maintainability.

C++ is selected as the primary implementation language because the application is expected to process large images and perform potentially intensive pixel-level operations while maintaining a responsive graphical interface.

Qt 6 is selected as the GUI framework because it provides mature cross-platform desktop application infrastructure for Windows, macOS, and Linux and integrates naturally with the selected C++ implementation language.

CMake is selected as the build system because it provides cross-platform build configuration and is suitable for C++ development and continuous integration.

The Core Library should use standard C++ wherever practical and should remain independent from Qt-specific GUI concepts.

The intended dependency direction is:

```text
GUI (Qt 6)
    │
    ▼
Core Library (C++)
    │
    ▼
Image Processing Backend
    │
    ▼
Image I/O
```

Qt-specific GUI concepts should not be introduced into the Core Library unless there is a clear technical reason to do so.

This technology selection applies to the initial implementation and may be revisited later only if implementation or experimental evidence demonstrates a substantial technical reason to change it.

## Algorithm — Current Hypothesis

The initial investigation will use a simple chromaticity measure:

```text
C = max(R,G,B) - min(R,G,B)
```

and a luminance estimate:

```text
L = 0.2126R + 0.7152G + 0.0722B
```

The first experiments will investigate whether chromaticity can be classified as unwanted according to both luminance and chromaticity.

A simple threshold model is only a starting hypothesis.

The intended direction is a decision function of the form:

```text
C > f(L)
```

where `f(L)` represents the maximum tolerated chromaticity at a given luminance.

The final mathematical model has not yet been determined.

## White Reference

The application should support two initial methods for defining the document white:

1. Automatic estimation from bright regions of the image.
2. Manual selection using a color picker.

The exact statistical method for estimating the white reference remains an open research question.

## Processing Modes

The initial design includes two conceptually different operations:

### Chromaticity Removal

Pixels classified as unwanted chromatic artifacts are removed according to the selected decision model.

### Neutralization

Chromaticity is reduced toward the neutral axis rather than simply replacing the pixel with white.

Neutralization is experimental and must remain clearly distinct from removal.

## Dark Content Protection

The algorithm must explicitly protect dark pixels.

As luminance decreases, stronger evidence of chromaticity should be required before a pixel is classified as an artifact.

This is intended to reduce damage to:

* text;
* thin lines;
* small details;
* halftones;
* degraded characters.

## User Transparency

The application should make processing decisions inspectable.

The user should be able to inspect an individual pixel and see:

```text
RGB
Luminance
Chromaticity
Decision
```

The diagnostic view should make it possible to understand which pixels would be removed and why.

## Testing Strategy

A public test dataset will be created containing examples such as:

* warm lighting;
* cool lighting;
* yellow paper;
* brown stains;
* blue stains;
* reflections;
* adjacent-page color contamination;
* thin black text;
* black text under a color cast;
* halftone content;
* gray paper;
* dark images.

Synthetic test images should also be created where the expected result is known.

Quality evaluation must separately consider:

1. Artifact removal.
2. Preservation of legitimate black content.

Producing a whiter image is not, by itself, a measure of success.

## Image Processing Backend

ImageMagick is a candidate for the initial implementation.

The application should not expose ImageMagick-specific concepts throughout the Core.

The intended abstraction is:

```text
ImageProcessor
    │
    └── ImageMagickProcessor
```

Other backends may be added later.

The choice of image-processing backend remains open and will be evaluated separately from the Core architecture.

## GUI Technology

Qt 6 is the selected GUI framework for the initial implementation.

The selection is based on:

* large image handling;
* zoom and pan requirements;
* cross-platform desktop support;
* integration with the C++ Core Library;
* mature application infrastructure;
* Windows support;
* macOS support;
* Linux support;
* maintainability;
* open-source ecosystem.

Qt/Python and Avalonia/.NET were considered as alternatives but are not selected for the initial implementation.

The GUI framework must remain an implementation layer and must not define the mathematical model or processing logic of Dechroma.

## Current Decisions

### DEC-001 — Specialized scope

Dechroma will focus on unwanted chromaticity in documents expected to be monochrome.

### DEC-002 — Algorithm independent from GUI

The processing algorithm will reside in a Core Library independent of the GUI.

### DEC-003 — Algorithm independent from ImageMagick

ImageMagick may be used as a backend, but it must not define the Core's architecture or mathematical model.

### DEC-004 — User-visible processing decisions

The application must provide mask and diagnostic views so the user can inspect the algorithm's decisions.

### DEC-005 — Project license

Dechroma is licensed under the Apache License, Version 2.0.

The SPDX identifier for the project license is:

```text
Apache-2.0
```

The complete license text is stored in the repository root as `LICENSE`.

The project may be used, modified, distributed, and incorporated into other software according to the terms of the Apache License, Version 2.0.

Third-party dependencies and their respective licenses must be documented separately as they are introduced into the project.

### DEC-006 — Initial technology stack

The initial Dechroma implementation will use:

```text
C++
Qt 6
CMake
```

C++ is the primary implementation language.

Qt 6 is the GUI framework.

CMake is the build system.

The Core Library will remain independent from Qt-specific GUI concepts wherever practical.

This decision applies to the initial implementation and may only be revisited if technical or experimental evidence demonstrates a substantial reason to change the technology stack.

## Open Decisions

The following decisions remain open:

* Image representation.
* Color model and color-space conventions.
* Final chromaticity metric.
* White-reference estimation method.
* Decision-curve representation.
* Neutralization algorithm.
* Image-processing backend.
* Parameter-file format.
* Distribution and packaging strategy.
* Dependency management strategy.
* Minimum supported operating-system versions.
* Continuous integration strategy.

The final chromaticity metric, white-reference method, decision curve, and neutralization algorithm are research questions for Phase 1 rather than technology-selection decisions.

## Current Phase

**Phase 0 — Project Definition and Architecture**

### Current objectives

1. Define the technical architecture.
2. Define the Core/GUI/backend interfaces.
3. Define the initial repository structure.
4. Define the image representation and memory strategy.
5. Establish the development and testing workflow.
6. Define the dependency and third-party licensing strategy.
7. Establish minimum supported platform versions.
8. Define the initial continuous integration strategy.
9. Document the architectural decisions required before implementation begins.

## Next Phase

**Phase 1 — Mathematical Model of Chromaticity**

The next phase will investigate how chromaticity should be represented and measured for the specific problem of monochrome document photographs and scans.

The research should compare simple RGB-based measures with alternative color representations and determine which characteristics are most useful for distinguishing unwanted chromaticity from legitimate dark content.

The research must also establish a test methodology capable of measuring both unwanted chromaticity removal and preservation of legitimate black content.

## Known Risks

### Risk 1 — Destruction of black content

An aggressive chromaticity detector may remove or alter legitimate dark document content.

This is the primary technical risk.

### Risk 2 — White-balance contamination

A photograph of white paper may contain a global color cast. A simple chromaticity threshold could incorrectly classify the entire page as contaminated.

### Risk 3 — Overfitting

An algorithm optimized for a small set of document photographs may fail under different lighting, cameras, papers, or scanning conditions.

### Risk 4 — Excessive algorithmic complexity

Adding spatial filters and multiple heuristics too early could make the algorithm difficult to explain, test, and reproduce.

The initial model should therefore remain as simple and measurable as possible.

### Risk 5 — Large-image performance

Document photographs and scans may contain tens or hundreds of millions of pixels.

The application must remain responsive while displaying and processing large images without requiring excessive memory.

### Risk 6 — Cross-platform differences

The application must produce consistent results across Windows, macOS, and Linux while accounting for differences in graphics APIs, image codecs, file systems, and distribution mechanisms.

## Development Principle

The project should follow this cycle:

```text
Hypothesis
    ↓
Implementation
    ↓
Test Dataset
    ↓
Measurement
    ↓
Analysis
    ↓
Revision
```

Algorithmic complexity should be added only when experimental evidence demonstrates that the simpler model is insufficient.

## Status History

### 2026-10-03

Initial project definition established.

README specification established.

Project Status document created.

Apache License 2.0 selected as the project license.

Initial technology stack selected:

* C++;
* Qt 6;
* CMake.

Repository structure not yet implemented.

GitHub development workflow not yet established.
