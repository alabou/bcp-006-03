# \[Work In Progress\] AMWA BCP-006-03: NMOS With H.265

[![Lint Status](https://github.com/AMWA-TV/bcp-006-03/workflows/Lint/badge.svg)](https://github.com/AMWA-TV/bcp-006-03/actions?query=workflow%3ALint)
[![Render Status](https://github.com/AMWA-TV/bcp-006-03/workflows/Render/badge.svg)](https://github.com/AMWA-TV/bcp-006-03/actions?query=workflow%3ARender)

This repository holds the source for this Specification, part of the family of [Networked Media Open Specifications](https://specs.amwa.tv/nmos) from the [Advanced Media Workflow Association](https://amwa.tv)

<!-- INTRO-START -->

### What does it do?

- Enables Registration, Discovery, and Connection Management of H.265 Endpoints using the AMWA IS-04, IS-05 and IS-11 NMOS Specifications.

### Why does it matter?

- It helps ensure consistency between implementations offering NMOS control for H.265 endpoints.

### How does it work?

- It specifies what resources and attributes MUST be provided when using NMOS with [H.265][] streams.
- It specifies requirements for SDP when it is used with a transport supporting SDP transport files.
- It provides examples of how to use IS-04, IS-05 and IS-11 in the context of H.265 streams - specifically H.265 encapsulated in RTP as described in IETF [RFC-7798][]. It also describe to some extents how to use H.265 with RTP and other transports that encapsulate the H.264 video streams in an MPEG2-TS transport stream.

[H.265]: https://www.itu.int/rec/T-REC-H.265 "High efficiency video coding"
[RFC-7798]: https://tools.ietf.org/html/rfc7798 "RTP Payload Format for High Efficiency Video Coding (HEVC)"

<!-- INTRO-END -->

## Getting started

There is more information about the NMOS Specifications and their GitHub repos at <https://specs.amwa.tv/nmos>.

