# Dechroma

Dechroma is an open-source, cross-platform application designed for one specific task: removing unwanted chromaticity from photographs and scans of documents that are expected to contain only black and white information.

A photographed or scanned document may contain unwanted color caused by lighting, paper discoloration, reflections, camera characteristics, or other photographic artifacts. Dechroma uses color information to identify pixels that are likely to be unwanted chromatic artifacts while attempting to preserve legitimate black content.

The fundamental assumption is simple:

> The document is monochrome. Black and white are information; unwanted chromaticity is an artifact.

Because of this assumption, Dechroma is intentionally specialized.

## What Dechroma is

* A tool for cleaning unwanted color from monochrome document images.
* Designed for photographs and scans of pages and similar documents.
* Focused on preserving black content while removing unwanted chromatic artifacts.
* Designed to make its processing decisions visible and inspectable.
* Open source and intended to run on Windows, macOS, and Linux.

## What Dechroma is not

Dechroma is not intended to be:

* A general-purpose image editor.
* An OCR application.
* A document restoration suite.
* A perspective correction or deskewing tool.
* A shadow or lighting correction system.
* A binarization tool.
* A generic noise or stain removal application.
* A complete document-processing suite.

These functions may be useful in other applications, but they are outside the core purpose of Dechroma.

## Design principle

Dechroma should not hide its decisions from the user.

The application is intended to provide visual feedback showing:

* the original image;
* the processed result;
* the chromaticity mask;
* diagnostic information about individual pixels.

The goal is not simply to produce a "whiter" page. The goal is to remove unwanted chromatic artifacts while preserving the document's legitimate black information.

## Project status

Dechroma is currently in the early development and research stage.

The initial work focuses on determining a robust and explainable mathematical model for distinguishing unwanted chromaticity from legitimate dark content under different photographic and scanning conditions.

The image-processing backend is an implementation detail. The underlying model should remain independent of any particular image-processing library.

## License

Dechroma is licensed under the Apache License, Version 2.0.

See the `LICENSE` file for the complete license text.

SPDX-License-Identifier: Apache-2.0


