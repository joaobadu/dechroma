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

## GUI Technology

The GUI framework has not yet been selected.

Candidates currently under consideration include:

* Qt/C++;
* Qt/Python;
* Avalonia/.NET;
* other suitable cross-platform frameworks.

The decision should consider:

* large image handling;
* zoom and pan performance;
* TIFF support;
* Windows support;
* macOS support;
* Linux support;
* distribution;
* maintenance;
* open-source ecosystem.

## Current Decisions

### DEC-001 — Specialized scope

Dechroma will focus on unwanted chromaticity in documents expected to be monochrome.

### DEC-002 — Algorithm independent from GUI

The processing algorithm will reside in a Core Library independent of the GUI.

### DEC-003 — Algorithm independent from ImageMagick

ImageMagick may be used as a backend, but it must not define the Core's architecture or mathematical model.

### DEC-004 — User-visible processing decisions

The application must provide mask and diagnostic views so the user can inspect the algorithm's decisions.

## Open Decisions

The following decisions remain open:

* Final programming language.
* GUI framework.
* Image representation.
* Color model.
* Final chromaticity metric.
* White-reference estimation method.
* Decision-curve representation.
* Neutralization algorithm.
* Image-processing backend.
* Project license.
* Parameter-file format.
* Distribution and packaging strategy.

## Current Phase

**Phase 0 — Project Definition and Architecture**

### Current objectives

1. Define the technical architecture.
2. Select the development technology.
3. Define the Core/GUI/backend interfaces.
4. Define the initial repository structure.
5. Choose the open-source license.
6. Establish the development and testing workflow.

## Next Phase

**Phase 1 — Mathematical Model of Chromaticity**

The next phase will investigate how chromaticity should be represented and measured for the specific problem of monochrome document photographs and scans.

The research should compare simple RGB-based measures with alternative color representations and determine which characteristics are most useful for distinguishing unwanted chromaticity from legitimate dark content.

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

Repository structure not yet implemented.

GitHub workflow not yet established.
