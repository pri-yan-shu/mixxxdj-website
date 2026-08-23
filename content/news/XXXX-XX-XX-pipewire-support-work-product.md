title: "GSoC 2026: Add PipeWire support"
authors: Priyanshu
tags: gsoc, gsoc-2026, development, PipeWire
date: XXXX-XX-XX 12:00:00

Disclaimer: *The blog post primarily serves as the documentation for the [Google Summer of Code](https://summerofcode.withgoogle.com/) 2026 project: "PipeWire support for Mixxx".*

## Overview
This project adds PipeWire audio API support to Mixxx for Linux platform.
PipeWire is an audio API for linux which exposes hardware sound devices and applications as nodes, their inputs/outputs as ports and their connections as links. This project's proposal can be found [here](https://github.com/mixxxdj/proposals/pull/19).

## Work done

| Pull Request | Title | Status | Description |
| #16544 | SoundManager refactor | Merged | Precursor to implementing PipeWire support |
| #16590 | [GSoC] [WIP] PipeWire support | Merged | Adds support for PipeWire API |
| #16707 | Initialize/deinitialize SoundDeviceEnumerator on apiComboBox change | Merged | Handle PipeWire server disconnects/restarts |
| #16712 | Improve SoundDevice and Channel naming on Sound Hardware preference page for PipeWire API | Merged | Display more appropriate sound device and its ports names |
| #16722 | PipeWire link hotplug | Merged | Configure/Unconfigure Mixxx inputs/outputs on external link creation/destruction |
| #16812 | Add option to force requested quantum and samplerate | Under review | Add option to force PipeWire server to process requested quantum and samplerate |
| #16834 | Add QSlider for PipeWire hardware volume control | In progress | Allow setting hardware device volume from Mixxx UI |
| #16918 | Implement Pipewire default device | Under review | Add a proxy device for default PipeWire source/sink |

*`Merged` implies that the PR is merged, `Under review` implies work is done, and is under review, near completion and `In progress` implies that there is work still left to do.*


## Pending work
- Currently PipeWire does not work with network broadcasting. Trigger PipeWire audio callbacks from network clock, and implement audio buffering.

- Instead of the planned unified model where Mixxx internal UI is updated according to external PipeWire routing events, we have 2 separate modes where the internal UI is enabled/disabled. One goal is to extend the Mixxx UI so it can accommodate the PipeWire supported scenario of configuring multiple audio devices (nodes in PipeWire terminology) onto a single Mixxx input/output, and unify the two modes, so that the UI is updated on external routing changes while still allowing the user to use the internal UI to route.

## Future Roadmap
- Currently Mixxx API is coded according to the PortAudio API, and PipeWire code adapts to that API, leading to boilerplate. It would be nice to refactor that into an architecture which is sufficient for both API.
- Quantify the latency improvements of native PipeWire API over the ALSA and JACK API on PipeWire systems, through PipeWire compatibility for ALSA and JACK.
