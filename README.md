# Explainer for the Auto-Generated Captions API

This proposal is an early design sketch by Yash Raj Bharti to describe the problem below and solicit
feedback on the proposed solution. It has not been approved to ship in Chrome.

## Proponents

- Yash Raj Bharti, Open Source Contributor, Liquid Galaxy (Google Open Source) Mentor

## Participate

- [GitHub Repository for Feedback](https://github.com/yashrajbharti/auto-generate-captions-video/issues)

## Table of Contents

<!-- START doctoc generated TOC -->
- [Introduction](#introduction)
- [Goals](#goals)
- [Non-goals](#non-goals)
- [User research](#user-research)
- [Use cases](#use-cases)
  - [Use case 1](#use-case-1)
  - [Use case 2](#use-case-2)
- [Potential Solution](#potential-solution)
  - [How this solution would solve the use cases](#how-this-solution-would-solve-the-use-cases)
    - [Use case 1](#use-case-1-1)
    - [Use case 2](#use-case-2-1)
- [Detailed design discussion](#detailed-design-discussion)
  - [Tricky design choice #1](#tricky-design-choice-1)
  - [Tricky design choice #2](#tricky-design-choice-2)
- [Considered alternatives](#considered-alternatives)
  - [Alternative 1](#alternative-1)
  - [Alternative 2](#alternative-2)
- [Stakeholder Feedback / Opposition](#stakeholder-feedback--opposition)
- [References & acknowledgements](#references--acknowledgements)
<!-- END doctoc generated TOC -->

## Introduction

Only **0.5% of web videos include a `<track>` tag** (source: *State of the Web 2024 Report*), leaving a majority of online video content inaccessible for individuals who rely on captions. This project proposes the `auto-generate` attribute to be added to the `<track>` element, enabling browsers to generate captions automatically. This solution aims to improve web accessibility and encourage widespread adoption of captions.

## Goals

- Address the **accessibility gap** by promoting captions as a default feature on the web.
- Enable developers to use an `auto-generate` attribute with the `<track>` element, allowing captions to be generated automatically.
- Simplify the process of captioning for content creators, reducing manual effort.
- Create a standardized approach to auto-generated captions, ensuring compatibility across browsers.

## Non-goals

- Replace manually created captions or subtitles.
- Create a proprietary solution that only works with specific browsers or platforms.
- Focus on live video streams (future scope).

## User research

The proposal is informed by research indicating that captions improve content comprehension, user engagement, and accessibility for people with hearing impairments and non-native speakers. The **0.5% adoption rate** of `<track>` tags highlights the need for a low-effort solution for developers.

## Use cases

### Use case 1

A small business uploads product videos to their website but lacks the resources to create captions manually. By adding a `<track auto-generate="en">` element, they can ensure captions are generated automatically, enhancing accessibility and SEO.

### Use case 2

An educational platform hosts a library of lecture videos. By using the `auto-generate` attribute, they can provide captions in multiple languages without manually transcribing each lecture.

## Potential Solution

### Solution Details

The `auto-generate` attribute will be added to the `<track>` element as follows:

```html
<video controls>
  <source src="example.mp4" type="video/mp4">
  <track kind="subtitles" auto-generate="en" label="English" />
</video>
```

When the browser encounters the `auto-generate` attribute, it uses built-in speech recognition and language models to generate captions dynamically. These captions are displayed as if they were part of the original `<track>` element.

### How this solution would solve the use cases

#### Use case 1

Small businesses can ensure video captions are available without hiring transcription services.

```html
<video controls>
  <source src="product-demo.mp4" type="video/mp4">
  <track kind="subtitles" auto-generate="en" label="English" />
</video>
```

#### Use case 2

Educational platforms can provide multi-language captions using the same attribute.

```html
<video controls>
  <source src="lecture.mp4" type="video/mp4">
  <track kind="subtitles" auto-generate="es" label="Spanish" />
</video>
```

## Detailed design discussion

### Tricky design choice #1

How should browsers handle the `auto-generate` attribute for unsupported languages?

Solution: Fallback to the default language or display a warning to the user.

### Tricky design choice #2

How can we prevent abuse or inaccurate captions from degrading the user experience?

Solution: Provide an optional `confidence-threshold` attribute to control caption quality.

## Considered alternatives

### Alternative 1

Add the `auto-generate` attribute directly to the `<video>` element.

Rejected: Lacks flexibility for multi-language support.

### Alternative 2

Leave caption generation to external tools.

Rejected: Adds complexity for developers and excludes dynamic web content.

## Stakeholder Feedback / Opposition

- **Positive Feedback**: Accessibility advocates emphasize the importance of lowering barriers to captioning.
- **Negative Feedback**: Some concerns about performance impacts for large video libraries.

## References & acknowledgements

Many thanks for valuable feedback and advice from:

- [Thomas Steiner](https://twitter.com/tomayac), Google DevRel
- The Chrome Team
- The State of the Web Report 2024

---
