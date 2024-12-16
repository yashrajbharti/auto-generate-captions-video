# Explainer for Auto-Generated Captions API

This proposal is an early design sketch by Yash Raj Bharti to describe the problem below and solicit feedback on the proposed solution. It has not been approved to ship in Chrome.

## Proponents

- Yash Raj Bharti, Open Source Contributor, Liquid Galaxy (Google Open Source) Mentor

## Participate

- [GitHub Repository for Feedback](https://github.com/yashrajbharti/auto-generate-captions-video/issues)

## Table of Contents

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
- [Feedback Q&A](#feedback-qa)
- [References & acknowledgements](#references--acknowledgements)

## Introduction

Only **0.5% of web videos include a ********`<track>`******** tag** (source: *[The Web Almanac by HTTP Archive 2024 Report](https://almanac.httparchive.org/en/2024/accessibility#audio-and-video)*), leaving a majority of online video content inaccessible for individuals who rely on captions. This project proposes a **built-in UI option in video elements** that enables browsers to auto-generate captions dynamically when a `<track>` element is missing. This solution aims to improve web accessibility without requiring explicit authoring changes.

## Goals

- Address the **accessibility gap** by promoting captions as a default feature on the web.
- Provide an implicit browser feature to generate captions dynamically when no `<track>` element exists.
- Simplify the process of captioning for users, reducing manual effort for developers.
- Allow user agents (UAs) to innovate and compete in improving the user experience.

## Non-goals

- Replace manually created captions or subtitles.
- Standardize the specific implementation of auto-generated captions across browsers.
- Focus on live video streams (future scope).

## User research

The proposal is informed by research indicating that captions improve content comprehension, user engagement, and accessibility for people with hearing impairments and non-native speakers. The **0.5% adoption rate** of `<track>` tags highlights the need for a low-effort solution that works automatically.

## Working Example

Using web technologies (HTML, CSS, and JS) combined with Whisper API models by HuggingFace's transformer.js, here's a working example of [auto-generate captions](https://yashrajbharti.github.io/captions-on-the-fly/src/example.html) from this [GitHub repo for Captions on the Fly](https://github.com/yashrajbharti/captions-on-the-fly).

## Use cases

### Use case 1

A small business uploads product videos to their website but lacks the resources to create captions manually. By enabling the built-in captions option in the browser UI, users can generate captions dynamically, improving accessibility and SEO.

### Use case 2

An educational platform hosts a library of lecture videos. By leveraging the browser’s built-in captioning option, viewers can enable captions in their preferred language without requiring the platform to provide them manually.

## Potential Solution

### Solution Details

Instead of introducing an explicit `autogenerate` attribute in the `<track>` element, this proposal suggests that browsers implement an **implicit UI feature** in the video player. This feature allows users to opt-in for auto-generated captions when no `<track>` element is present.

For example:

```html
<video controls>
  <source src="example.mp4" type="video/mp4">
</video>
```

When the browser detects the absence of a `<track>` element, it displays a captions button in the video player controls. Clicking the button triggers the browser’s built-in speech recognition and language models to generate captions dynamically.

### How this solution would solve the use cases

#### Use case 1

Small businesses can rely on the browser’s built-in captions feature to make their video content accessible without hiring transcription services.

#### Use case 2

Educational platforms can provide multi-language captions through the browser’s built-in UI, reducing the need for manual transcription efforts.

## Detailed design discussion

### Tricky design choice #1

How should browsers handle unsupported languages in the auto-generated captions feature?

Solution: Fallback to the default language or display a warning to the user that the selected language is unavailable.

### Tricky design choice #2

How can we prevent abuse or inaccurate captions from degrading the user experience?

Solution: Provide visual indicators (e.g., a "Generated Captions" badge) and allow users to toggle the feature on or off easily.

## Considered alternatives

### Alternative 1

Add the `autogenerate` attribute directly to the `<video>` element.

Rejected: Lacks flexibility for multi-language support and shifts responsibility to developers rather than empowering users.

### Alternative 2

Leave caption generation entirely to external tools.

Rejected: Adds complexity for developers and excludes dynamic web content.

## Stakeholder Feedback / Opposition

- **Positive Feedback**: Accessibility advocates emphasize the importance of lowering barriers to captioning.
- **Negative Feedback**: Some concerns about performance impacts for large video libraries.

## Feedback Q&A

### 1. **What if this leads to even fewer people manually creating subtitles?**

- **Answer:** While manual captions are often more accurate and tailored, the built-in captions feature is designed as a complementary tool, not a replacement. It ensures accessibility where no captions exist, providing an immediate benefit. To encourage manual captioning, browsers could display a notification recommending content creators add their own captions.

### 2. **Does it make sense for every user who watches a video to run the same subtitle creation steps repeatedly, or would it be better to run this once on the server and deliver the same subtitles to everyone?**

- **Answer:** Server-side generation ensures consistency and reduces computational load for users, but client-side generation offers flexibility for dynamic or personalized content. A hybrid approach—generating and caching captions locally—could optimize performance.

### 3. **What about videos on the web where it’s a live stream?**

- **Answer:** For live-streamed videos, real-time caption generation is essential. The proposed built-in feature is designed to handle such cases, ensuring accessibility for live events.

### 4. **How would it deal with a translated language that has very long words?**

- **Answer:** Browsers can handle text overflow by applying word wrapping, truncation, or adjustable font sizes. Developers could also provide styling options for captions to manage layout constraints.

### 5. **What if the generated subtitles don't fit the originally spoken parts?**

- **Answer:** The feature can include synchronization buffers and confidence thresholds to align captions with spoken content and suppress low-quality captions.

### 6. **Why are we not adding auto captions to the track element?**

- **Answer:** The `<track>` element by definition says, “Captions exist for this video, and this element provides them.” Introducing a case where a `<track>` element indicates, “Captions don’t exist, but the UA should generate them,” contradicts this purpose. Instead, leveraging the absence of a `<track>` element as a signal for auto-generation better aligns with the intent of the standard.

## References & acknowledgements

Many thanks for valuable feedback / advice / support from:

- [Thomas Steiner](https://github.com/tomayac), Google
- [Adam Argyle](https://github.com/argyleink), Google
- [Mike Smith](https://github.com/sideshowbarker), W3C
- [The Web Almanac by HTTP Archive 2024](https://almanac.httparchive.org/en/2024/accessibility#audio-and-video)

