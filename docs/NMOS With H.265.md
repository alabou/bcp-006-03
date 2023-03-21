# AMWA BCP-006-03: NMOS With H.265

> WORK IN PROGRESS ... DOCUMENT UNDER CONSTRUCTION

{:.no_toc}

- A markdown unordered list which will be replaced with the ToC, excluding the "Contents header" from above
{:toc}

_(c) AMWA 2023, CC Attribution-NoDerivatives 4.0 International (CC BY-ND 4.0)_

![NMOS logo](images/NMOS-logo.png)

> ## Instructions
>
> Add your content as (GitHub Flavoured) Markdown documents.
>
> Put diagrams (ideally PNG with encapsulated draw.io source) in the `images/` sub-directory.
>
> Follow the [Style Guide](Style%20Guide.md).
>
> Make a bulleted list of documents in `README.md` in this directory.
> 
> Set the repo name used to get the Lint and Render status in the top-level `README.md` (four changes needed).
>
> Set the value of `amwa_id` in `.render/_config.yml` to the AMWA-assigned ID.

## Introduction

H.265 is a technology standardized in Rec. [ITU-T H.265][H.265] | ISO/IEC 23008 for video contribution at high compression rate and video quality.
A companion RTP payload format specification was developed through the IETF Payloads working group, IETF [RFC 7798][RFC-7798] for the transport of an H.265 bitstream over RTP.

The BCP-006-03 specification includes support for bitstreams that are compliant with the clauses of the main document and annexes A, B, C, D and E of the [ITU-T H.265][H.265] | ISO/IEC 23008 specification. It excludes support for bitstreams that are compliant with other annexes of the specification.
> Annex F (multi-layers extensions), Annex G (multiview high efficiency video coding), Annex H (scalable high efficiency video coding) and Annex I (3D high efficiency video coding) are not supported.

The [ITU H.222][H.222] specification and associated amendments describe the embedding of an H.265 stream in an MPEG2-TS transport stream. An RTP payload format specification for MPEG2-TS transport stream was developed through the IETF Payloads working group, IETF [RFC 2550][RFC-2550] for transport over RTP. Other normative documents may describe the requirements for the streaming of an MPEG2-TS transport stream over other non-RTP transports.

The [Society of Media Professionals, Technologists and Engineers][SMPTE] developed Standard [ST 2110-22][ST-2110-22] of the ST 2110 suite of protocols, which cover the end-to-end application use of constant bitrate compression for video over managed IP networks.
> Note that the definition of constant bitrate of ST 2110-22 is very strict "The video compression or the packetization of the video compression shall produce a constant number of bytes per frame. The packetization shall produce a constant number of RTP packets per frame.". This definition of constant bitrate is hereafter described as strict-CBR, using the H.265 definition of constant bitrate for CBR.

The [Video Services Forum][VSF] developed Technical Recommendation [TR-10-11][TR-10-11] and [TR-10-??][TR-10-??] of the IPMX suite of protocols, which cover the end-to-end application use of constant and variable bitrate compression for video, using the SMPTE ST 2110 and IPMX suite of protocols.

TR-10-11 and TR-10-?? mandate the use of the AMWA [IS-04][IS-04] and [IS-05][IS-05] NMOS Specifications in IPMX compliant systems.

AMWA IS-04 and IS-05 already have support for RTP transport and can signal the media type `video/H265` as defined in RFC 6184.

## Use of Normative Language

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY",
and "OPTIONAL" in this document are to be interpreted as described in [RFC 2119][RFC-2119].

## Definitions

The NMOS terms 'Controller', 'Node', 'Source', 'Flow', 'Sender', 'Receiver' are used as defined in the [NMOS Glossary](https://specs.amwa.tv/nmos/main/docs/Glossary.html).

The term 'strict-CBR' corresponds to the definition of constant bit-rate at section 4 "Video Compression and Packetization" of the ST 2110-22 standard.

The terms CBR and VBR are defined in the [ITU-T H.265][H.265] | ISO/IEC 23008 standard.

## H.265 IS-04 Sources, Flows and Senders

Nodes capable of transmitting H.265 video streams MUST have Source, Flow and Sender resources in the IS-04 Node API.

### Sources

The Source resource MUST indicate `urn:x-nmos:format:video` for the `format` attribute. 
- Source resources MAY be associated with many Flows at the same time. The Source is therefore unaffected by the use of H.265 compression.

### Flows

The Flow resource MUST indicate `video/H265` in the `media_type` attribute, and `urn:x-nmos:format:video` for the `format` attribute. This has been permitted since IS-04 v1.1. 
- H.265 Flow resources MAY be associated with many Senders at the same time through the Sender's `flow_id` attribute. The H.265 Flow is therefore unaffected by the use of a specific transport. 
- H.265 Flow resources MAY be associated with many Flows at the same time trough the Flow's `parents` attribute. The H.265 Flow is therefore unaffected by being the parent of some other Flows.

For Nodes implementing IS-04 v1.3 or higher, the following additional requirements on the Flow resource apply.

In addition to those attributes defined in IS-04 for all coded video Flows, the following attributes defined in the [Flow Attributes Register](https://specs.amwa.tv/nmos-parameter-registers/branches/main/flow-attributes/) of the [NMOS Parameter Registers][] are used for H.265.

These attributes provide information for Controllers and Users to evaluate stream compatibility between Senders and Receivers.

- [Components](https://specs.amwa.tv/nmos-parameter-registers/branches/main/flow-attributes/#components)
  The Flow resource MUST indicate the color (sub-)sampling, width, height and depth of the associated uncompressed picture using the `components` attribute. The `components` array values MUST correspond to the stream's active parameter sets values. A Flow MUST track the stream's current active parameter sets.

Informative note: ST 2110-22 does not require the `sampling` or `depth` SDP parameters. RFC 6184 does not define any such SDP parameters. The `sampling`, `width`, `height` and `depth` of the associated uncompressed picture must be derived from the H.265 active parameter sets by a Receiver.

- [Profile](https://specs.amwa.tv/nmos-parameter-registers/branches/main/flow-attributes/#profile)
The Flow resource MUST indicate the H.265 profile, which defines algorithmic features and limits that SHALL be supported by all decoders conforming to that profile. It SHALL comply with the stream's active parameter sets. The permitted `profile` values are strings, defined as per ITU-T Rec. H.265 Annex A

  - "Main" (Default if not specified in the SDP transport file)
  - "Main10"
  - "Main10Still"
  - "MainStill"
  - "Monochrome"
  - "Monochrome10"
  - "Monochrome12"
  - "Monochrome16"
  - "Main12"
  - "Main10-422"
  - "Main12-422"
  - "Main444"
  - "Main10-444"
  - "Main12-444"
  - "MainIntra"
  - "Main10Intra"
  - "Main12Intra"
  - "Main10Intra-422"
  - "Main12Intra-422"
  - "MainIntra-444"
  - "Main10Intra-444"
  - "Main12Intra-444"
  - "Main16Intra-444"
  - "MainStill-444"
  - "Main16Still-444"
  - "HighThroughput-444"
  - "HighThroughput10-444"
  - "HighThroughput14-444"
  - "HighThroughput16Intra-444"
  - "ScreenExtendedMain"
  - "ScreenExtendedMain10"
  - "ScreenExtendedMain-444"
  - "ScreenExtendedMain10-444"
  - "ScreenExtendedHighThroughput-444"
  - "ScreenExtendedHighThroughput10-444"
  - "ScreenExtendedHighThroughput14-444"

The Flow's `profile` attribute maps to the `profile-space`, `profile-id`, `profile-compatibility-indicator`, `interop-constraints` parameters of the SDP transport file. See section SDP format-specific parameters.

The Flow's `profile` attribute maps to the members profile_space, profile_idc, profile_compatibility_indication, progressive_source_flag, interlaced_source_flag, non_packed_constraint_flag, frame_only_constraint_flag and copied_44bits and  of the HEVC_video_descriptor of an MPEG2-TS transport stream. See section Multiplexed Flows.

- [Level](https://specs.amwa.tv/nmos-parameter-registers/branches/main/flow-attributes/#level)
 The Flow resource MUST indicate the H.265 level, which defines a set of limits on the values that may be taken by the syntax elements of an H.265 bitstream.  It SHALL comply with the stream's active parameter sets. The permitted `level` values are strings, defined as per ITU-T Rec. H.265 Annex A
  - "1"
  - "2", "2.1"
  - "3"
  - "3.1" (Default if not specified in the SDP transport file)
  - "4", "4.1"
  - "5", "5.1", "5.2"
  - "6", "6.1", "6.2"
  - "High-1"
  - "High-2", "High-2.1"
  - "High-3", "High-3.1"
  - "High-4", "High-4.1"
  - "High-5", "High-5.1", "High-5.2"
  - "High-6", "High-6.1", "High-6.2"
  - "High-8.5"

The Flow's `level` attribute map to the `level-id` and `tier-flag` parameters of the SDP transport file. See section SDP format-specific parameters.

The Flow's `level` attribute map to the members level_idc and tier_flag of the HEVC_video_descriptor of an MPEG2-TS transport stream. See section Multiplexed Flows.

Informative note: The Flow's `profile` and `level` attributes are always required. The SDP transport file `profile-space`, `profile-id`, `profile-compatibility-indicator`, `interop-constraints`, `level-id` and `tier-flag` parameters MAY be omitted when matching the default value.

- [Bit Rate](https://specs.amwa.tv/nmos-parameter-registers/branches/main/flow-attributes/#bit-rate)
  The Flow resource MUST indicate the target encoding bit rate (kilobits/second) of the H.265 bitstream. It SHALL comply with the stream's active parameter sets. The `bit_rate` integer value is expressed in units of 1000 bits per second, rounding up.

  Informative note: The H.265 bitstream is not required to transport HRD parameters such that an H.265 decoder may not know the actual target bitrate of a stream. There are bit rate limits imposed by the level of the coded bitstream. IS-11 may be used to constraint the Sender to a target bit rate compatible with the Receiver Capabilities.

 - [Constant Bit Rate](https://specs.amwa.tv/nmos-parameter-registers/branches/main/flow-attributes/#constant-bit-rate)
  The Flow resource MUST indicate if it operates in constant bit rate (CBR) mode or variable bit rate mode (VBR or other). When operating in constant bit rate mode the `bit_rate` corresponds to the constant encoding bit rate. Otherwise it corresponds to the maximum encoding bit rate. Since the default value of this attribute is `false`, a Flow MAY omit this attribute when using a variable bitrate mode.

Informative note: The maximum bit rate information relates to the codec profile / level limits and the HRD buffering model. The CBR versus VBR mode of operation of the encoder provide essential clues about the coded bitstream produced.

Informative note: For Flows compliant with ST 2110-22, the constant bit rate mode is more appropriately described as a strict-CBR mode where "The video compression or the packetization of the video compression shall produce a constant number of bytes per frame. The packetization shall produce a constant number of RTP packets per frame.". For other Flows, not compliant with ST 2110-22, it is the constant bit rate definition of of the H.265 specification that prevail.

Examples Flow resources are provided in [Examples](../examples/).

### Senders
#### RTP transport
##### SDP format-specific parameters

This section applies to a Sender with RTP transport directly associated to an H.265 Flow through the Sender's `flow_id` attribute.

Informative note: When an H.265 Flow is not directly associated with a Sender but with another Flow through the Flow's parents attribute, it does not have to provide format-specific attributes in the form of an SDP transport file. A Sender has such requirement only for the Flow directly associated with it.

The SDP file at the `manifest_href` MUST comply with the requirements of RFC 7798 in the [Usage in Declarative Session Descriptions](https://www.rfc-editor.org/rfc/rfc7798.html#section-7.2.3) mode of operation. The SDP Offer/Answer Model described in RFC 7798 is not supported. The `fmtp` source attribute as specified in Section 6.3 of RFC 5576 (Source-Specific Media Attributes in the Session Description Protocol) is not supported. The `tx-mode` parmeter of the SDP transport file SHALL always be set to SRST (Single RTP Stream Transport).

Additionally, the SDP transport file needs to convey, so far as the defined format-specific parameters allow, the same information about the stream as conveyed by the Source, Flow and Sender attributes defined by this specification and IS-04, unless such information is conveyed through in-band parameter sets.

Therefore:
- The `profile-space`, `profile-id`, `profile-compatibility-indicator`, `interop-constraints`, `level-id` and `tier-flag` format-specific parameters MUST be included with the correct value unless it corresponds to the default value.

- The `sprop-max-don-diff` format-specific parameters MUST be included with the correct value unless it corresponds to the default value.

- The `sprop-depack-buf-nalus`, `sprop-depack-buf-bytes` format-specific parameters SHOULD be included with the correct value if `sprop-max-don-diff` is not 0 unless it corresponds to the default value.

- The `sprop-vps`, `sprop-sps` and `sprop-pps` MUST always be included if the Sender `parameter_sets_transport_mode` property is `out_of_band` and SHOULD be included if the property is `in_and_out_of_band`.

The format-specific parameters declared in the "fmtp=" attribute of an SDP transport file except `sprop-vps`, `sprop-sps` and `sprop-pps` MUST comply with the stream's active parameter sets.

If the Sender meets the traffic shaping and delivery timing requirements specified for ST 2110-22, the SDP transport file MUST also comply with the provisions of ST 2110-22.

If the Sender meets the traffic shaping and delivery timing requirements specified for IPMX, the SDP transport file MUST also comply with the provisions of IPMX.

#### Other transports
#### Multiplexed Flows

## H.265 IS-04 Receivers
### RTP transport
### Other transports
### Multiplexed Flows

## H.265 IS-05 Senders and Receivers
### RTP transport
### Other transports
### Parameter Sets

## H.265 IS-11 Senders and Receivers
### RTP transport
### Other transports

## Controllers

[BCP-004-01]: https://specs.amwa.tv/bcp-004-01/ "AMWA BCP-004-01 NMOS Receiver Capabilities"
[H.265]: https://www.itu.int/rec/T-REC-H.265 "High efficiency video coding"
[H.222]: https://www.itu.int/rec/T-REC-H.222.0 "Generic coding of moving pictures and associated audio information: Systems"
[RFC-2119]: https://tools.ietf.org/html/rfc2119 "Key words for use in RFCs"
[RFC-7798]: https://tools.ietf.org/html/rfc7798 "RTP Payload Format for High Efficiency Video Coding (HEVC)"
[IS-04]: https://specs.amwa.tv/is-04/ "AMWA IS-04 NMOS Discovery and Registration Specification"
[IS-05]: https://specs.amwa.tv/is-05/ "AMWA IS-05 NMOS Device Connection Management Specification"
[NMOS Parameter Registers]: https://specs.amwa.tv/nmos-parameter-registers/ "Common parameter values for AMWA NMOS Specifications"
[TR-10-11]: https://vsf.tv/download/technical_recommendations/VSF_TR-10-11_2023-??-??.pdf ""
[TR-10-??]: https://vsf.tv/download/technical_recommendations/VSF_TR-10-??_2023-??-??.pdf ""
[VSF]: https://vsf.tv/ "Video Services Forum"
[SMPTE]: https://www.smpte.org/ "Society of Media Professionals, Technologists and Engineers"
[ST-2110-22]: https://ieeexplore.ieee.org/document/9893780 "ST 2110-22:2022 - SMPTE Standard - Professional Media Over Managed IP Networks: Constant Bit-Rate Compressed Video"
