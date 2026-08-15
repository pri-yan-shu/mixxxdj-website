title: "GSoC 2026: PipeWire support"
authors: Priyanshu (piri)
tags: gsoc, gsoc-2026, development, PipeWire
comments: yes
summary: Progress report on Google Summer of Code 2026 project to add PipeWire support to Mixxx on Linux
date: 2026-08-01 01:01:01

Hi, I'm Priyanshu, and for my GSoC 2026 project, me and @daschuer have been working on adding PipeWire support to Mixxx on Linux platform. This is a brief overview of all the progress that has been made. The proposal for this can be found [here](https://github.com/mixxxdj/proposals/pull/19).

---

## Wait a minute, what even is a pop wire?
Linux audio stack has moved through a plethora of audio APIs depending on the use cases, beginning with ALSA (at the bottom of linux audio stack), PulseAudio (a desktop audio centric API), JACK (a pro-audio oriented API with additional quality of life features like patchbay routing), and finally PipeWire, which addresses desktop as well as pro-audio use cases. It has compatibility with existing PulseAudio and JACK applications, and that's how Mixxx can run on PipeWire systems through its ALSA or JACK APIs.

One big plus of PipeWire is that it puts all programs consuming and producing audio at same abstraction level as hardware soundcards (similar to JACK). As a result, it is possible to route the output of Spotify or YouTube to Mixxx, then connect Mixxx's output to some other DSP program (like [easyeffects](https://github.com/wwmm/easyeffects)), and then output to any other program (even record the output to file using pw-record program). The possibilities are endless. One improvement over the JACK workflow is that you can use a PipeWire patchbay to manage Mixxx's routing entirely, bypassing Mixxx's own routing UI.

Historically Mixxx supported its audio APIs through PortAudio, which allowing a common implementation to work with different native audio APIs. PortAudio exposes the lowest common API between all different platforms, and Mixxx was missing out on a lot of features.

As PipeWire gains more popularity on linux, and more and more distributions having it as the default, Mixxx was lagging behind in the rich featureset provided by the API. With this project, Mixxx can now participate directly in the modern Linux audio stack without any compatibility layers.

---

## Features

### Integration with external patchbay
Mixxx supports routing with external patchbays like [qpwgraph](https://github.com/rncbc/qpwgraph). For that you need to check the "Sync with external patchbay" checkbox in Sound API preference page. This disables Mixxx preference page and configuration loading, so you can use the patchbay for connecting/disconnecting, and let patchbay automatically route Mixxx on startups. The automatic routing also handles programs start and end at runtime. With this you also have the ability to connect multiple inputs and outputs to and from Mixxx. With the sync option off, Mixxx behaves in a more traditional way, where Mixxx UI routes take precedence over patchbay routes, and they are removed once configuration is applied.

### Hardware volume control
The DJ controllers used along with Mixxx can have a dedicated soundcard, providing an audio interface with multiple inputs/outputs. For controllers which do expose hardware analog gain controls, we can control the gain of each input/output from the controls present on the controller, and now we can do the same from Mixxx UI itself. For audio input, this can improve the signal-to-noise ratio when analog signal is being converted to digital, and for audio outputs, we preserve more digital headroom, since we don't have to amplify audio digitally (which can cause clipping if boosted beyond the available digital range), and instead can use the analog amplification instead.

### Samplerate and buffer size negotiation
Since PipeWire lets multiple programs use a single soundcard, the server runs at a single quantum (buffer size) and sample rate, and any application either needs to agree to use the server-determined quantum/sample rate, or have PipeWire resample in between. Unlike JACK, where the server determines the sample rate and quantum and Mixxx adapts to them, with PipeWire Mixxx can either agree to use whatever quantum/sample rate PipeWire provides (and hence play cooperatively with the rest of the applications), or force its own quantum/sample rate. The latter can fail  or be overridden when another specialized application forces its own quantum/sample rate. In that case, you have to choose between running one of either program, you cannot have your cake and eat it too ;).

---

## Testing

You can try Mixxx with PipeWire by:
- Get a build of the main branch
- Run Mixxx with `--developer` flag
- Check the "Use PipeWire" checkbox in Sound API preference page

Report any issues, bugs, or general workflow/enhancement wishlist in the testing topic linked at the end.

---

# Links

- [Proposal](https://github.com/mixxxdj/proposals/pull/19) for this project.
- [Link](https://mixxx.zulipchat.com/#narrow/channel/267968-testing/topic/PipeWire.20testing/with/615558629) to the testing topic on Zulip.


