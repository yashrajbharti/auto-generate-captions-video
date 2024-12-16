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
- [Feedback Q&A](#feedback-qa)
- [References & acknowledgements](#references--acknowledgements)
<!-- END doctoc generated TOC -->

## Introduction

Only **0.5% of web videos include a `<track>` tag** (source: *[The Web Almanac by HTTP Archive 2024 Report](https://almanac.httparchive.org/en/2024/accessibility#audio-and-video)*), leaving a majority of online video content inaccessible for individuals who rely on captions. This project proposes the `autogenerate` attribute to be added to the `<track>` element, enabling browsers to generate captions automatically. This solution aims to improve web accessibility and encourage widespread adoption of captions.

## Goals

- Address the **accessibility gap** by promoting captions as a default feature on the web.
- Enable developers to use an `autogenerate` attribute with the `<track>` element, allowing captions to be generated automatically.
- Simplify the process of captioning for content creators, reducing manual effort.
- Create a standardized approach to auto-generated captions, ensuring compatibility across browsers.

## Non-goals

- Replace manually created captions or subtitles.
- Create a proprietary solution that only works with specific browsers or platforms.
- Focus on live video streams (future scope).

## User research

The proposal is informed by research indicating that captions improve content comprehension, user engagement, and accessibility for people with hearing impairments and non-native speakers. The **0.5% adoption rate** of `<track>` tags highlights the need for a low-effort solution for developers.

## Working Example

Using web technologies (HTML, CSS and JS) combined with whisper API models by HuggingFace's transformer.js, here's a working example of [autogenerate captions](https://yashrajbharti.github.io/captions-on-the-fly/src/example.html) from this [github repo for Captions on the fly](https://github.com/yashrajbharti/captions-on-the-fly). 

## Use cases

### Use case 1

A small business uploads product videos to their website but lacks the resources to create captions manually. By adding a `<track autogenerate="en">` element, they can ensure captions are generated automatically, enhancing accessibility and SEO.

### Use case 2

An educational platform hosts a library of lecture videos. By using the `autogenerate` attribute, they can provide captions in multiple languages without manually transcribing each lecture.

## Potential Solution

### Solution Details

The `autogenerate` attribute will be added to the `<track>` element as follows:

```html
<video controls>
  <source src="example.mp4" type="video/mp4">
  <track kind="subtitles" autogenerate="en" label="English" />
</video>
```

When the browser encounters the `autogenerate` attribute, it uses built-in speech recognition and language models to generate captions dynamically. These captions are displayed as if they were part of the original `<track>` element.

### How this solution would solve the use cases

#### Use case 1

Small businesses can ensure video captions are available without hiring transcription services.

```html
<video controls>
  <source src="product-demo.mp4" type="video/mp4">
  <track kind="subtitles" autogenerate="en" label="English" />
</video>
```

#### Use case 2

Educational platforms can provide multi-language captions using the same attribute.

```html
<video controls>
  <source src="lecture.mp4" type="video/mp4">
  <track kind="subtitles" autogenerate="es" label="Spanish" />
</video>
```

## Detailed design discussion

### Tricky design choice #1

How should browsers handle the `autogenerate` attribute for unsupported languages?

Solution: Fallback to the default language or display a warning to the user.

### Tricky design choice #2

How can we prevent abuse or inaccurate captions from degrading the user experience?

Solution: Provide an optional `confidence` attribute to control caption quality.

## Considered alternatives

### Alternative 1

Add the `autogenerate` attribute directly to the `<video>` element.

Rejected: Lacks flexibility for multi-language support.

### Alternative 2

Leave caption generation to external tools.

Rejected: Adds complexity for developers and excludes dynamic web content.

## Stakeholder Feedback / Opposition

- **Positive Feedback**: Accessibility advocates emphasize the importance of lowering barriers to captioning.
- **Negative Feedback**: Some concerns about performance impacts for large video libraries.

## Feedback Q&A

### 1. **What if this leads to even fewer people manually creating subtitles?**
   - **Answer:**  
     While manual captions are often more accurate and tailored, the `autogenerate` feature is designed as a complementary tool, not a replacement. The intent is to provide captions where none exist, improving accessibility immediately. To mitigate dependency on automation, the browser could display a message or badge indicating that captions are auto-generated, encouraging content creators to manually improve them when possible.

### 2. **Does it make sense for every user who watches a video to run the same subtitle creation steps repeatedly, or would it be better to run this once on the server and deliver the same subtitles to everyone?**
   - **Answer:**  
     Generating captions on the server could indeed save computational resources and ensure consistency across users. However, there are trade-offs:
       - **Server-side generation**: Requires content creators to implement additional infrastructure.
       - **Client-side generation**: Offers flexibility, especially for dynamic or user-specific content (e.g., captions based on regional dialects or personalization).  
     To balance this, the API could prioritize caching generated captions locally or storing them for reuse, reducing repetitive computation.

### 3. **What about videos on the web where it’s a live stream?**
   - **Answer:**  
     For live-streamed videos, storing captions on a server is not applicable, as the captions must be generated in real time. The `autogenerate` feature is essential for such cases, enabling immediate accessibility without requiring pre-existing infrastructure. Real-time generation ensures that live events remain inclusive to viewers who rely on captions.

### 4. **How would it deal with a translated language that has very long words?**
   - **Answer:**  
     Handling text overflow for translated captions is a valid concern. The `autogenerate` API can leverage browser styling rules, such as word wrapping or adjustable font sizes. Additionally, developers could specify optional attributes like `truncate` or `wrap` to control how captions are displayed. A fallback mechanism could also allow manual adjustments for languages with significantly different word lengths.

### 5. **What if the generated subtitles don't fit the originally spoken parts?**
   - **Answer:**  
     To address mismatches between captions and spoken content, the API could include:
       - A **synchronization buffer** to align captions better with audio tracks.
       - A confidence threshold attribute to prevent low-quality captions from being displayed.

## References & acknowledgements

Many thanks for valuable feedback / advice / support from:

- [Thomas Steiner](https://github.com/tomayac), Google
- [Adam Argyle](https://github.com/argyleink), Google
- The Chrome Team
- [The Web Almanac by HTTP Archive 2024](https://almanac.httparchive.org/en/2024/accessibility#audio-and-video)

---
