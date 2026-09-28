Türkçe: [README.tr.md](README.tr.md)

# RemoteBite: Private, Offline AI Voice Control for Your Smart TV

## Summary

**RemoteBite** is a smartphone TV remote whose main feature is **on-device AI voice control**. The user speaks naturally, for example *"mute and go to channel 5 then open the guide"* or *"connect to my TV"*, and the app understands the request, breaks it into the right sequence of remote-control steps, sends them to the TV and confirms by voice.

All speech recognition and language understanding run **on the phone**. After a one-time model download the voice feature works fully offline: no audio leaves the device, no usage data goes to a server and there are no server costs. Understanding is handled by a **hybrid engine**: fast rules first, then a small local language model (**LFM2.5-350M**) for phrasing the rules cannot handle. An optional offline **Whisper** speech recognizer can replace the phone's built-in one.

The end goal is **"zero-click" control**: every action in the app, including finding and connecting to a TV, waking it from standby, changing channels by name, changing any app setting and moving between screens, can be done by voice alone. The idea also includes a richer remote-control feature set (TV manager, Wake-on-LAN, smart channel buttons, touchpad and keyboard, named input sources) so that voice and touch cover the same ground.

A working prototype of the voice core exists. Zero-click control is the next milestone.

## Problem

- **Physical and on-screen remotes rely on small buttons.** Channel digits, colour keys, input selection and settings menus all need precise taps and a clear view of the screen. Users who struggle with small buttons or with seeing them have no real alternative.
- **Voice remotes usually depend on the cloud.** Mainstream voice assistants send audio to remote servers. That raises privacy concerns, needs an internet connection and adds running costs for whoever operates the service.
- **Simple voice commands are not enough.** Real requests are often compound ("mute and switch to channel 5"), use names instead of numbers ("put on Show TV") or state a target instead of a step ("volume to 20"). A plain keyword matcher either fails or picks the wrong single action.
- **Voice rarely reaches the whole app.** Even where voice exists, it usually covers only basic remote keys. Connecting to a TV, managing several TVs, changing app settings and moving between screens still need touch.
- **TVs do not report their state.** The common remote protocol cannot read back the current volume, channel or power state, so commands like "set volume to 20" cannot be carried out by simply asking the TV.

## Proposed Solution

1. **On-device voice pipeline.** Speech is turned into text on the phone (the phone's built-in recognizer, or an optional offline Whisper model), understood on the phone, executed against the TV and confirmed with spoken feedback.
2. **Hybrid understanding.** A layered engine first checks corrections it has learned from the user, then applies rule tables (18 language keyword tables with exact and fuzzy matching across several speech hypotheses), and only then asks the local language model. This keeps common commands instant and still handles free-form phrasing.
3. **"The model never executes anything."** The language model only picks an intent and fills in its details. A deterministic planner turns that into concrete low-level steps (key presses, digit dialling, connecting, changing a setting), and an executor runs them one by one. This keeps behaviour predictable and testable.
4. **An intent catalog that covers the whole app.** The vocabulary grows from 21 TV intents to 41, covering TV control, connection management, extra remote keys, app navigation, settings, model management and volume calibration.
5. **Compound commands and follow-up questions.** Utterances are split into parts, each part is understood separately and the steps run in order. The assistant can ask for confirmation or clarification and take the answer by voice.
6. **Settings by voice.** Every app setting can be changed by voice, with validation, spoken confirmation and an extra confirmation step for risky changes such as switching the app language.
7. **Zero-click availability.** Voice works on every screen, and the voice path can scan for, connect to and wake TVs itself. From app launch, *"connect to my TV"* leads through discovery and pairing to a ready remote without touching the screen.
8. **State estimation instead of readback.** Because the TV cannot report its state, the app tracks an estimated volume, mute state and recently dialled channels. The user can recalibrate by voice ("the volume is 35").
9. **A full remote alongside voice.** A TV manager with saved TVs, Wake-on-LAN, configurable smart channel buttons, a touchpad and keyboard tab, a named input-source picker, manual IP and VPN modes, haptics and auto-reconnect.

## How It Works

### Voice pipeline

1. **Listen.** The user taps the mic or says a wake phrase. The app captures audio on the device. A voice-activity gate with a short pre-roll buffer decides when speech starts and ends, after a brief calibration to the room's noise level. A live mic-level bar shows the user that they are being heard.
2. **Transcribe.** The phone's built-in on-device recognizer (tuned for dictation, preferring on-device mode) produces several alternative transcripts with confidence scores. If the user turns on **offline speech recognition**, a downloaded Whisper model transcribes instead.
3. **Understand.** The hybrid engine turns each part of the utterance into an intent with details (for example *change channel, name = "Show TV"*) and a confidence score. It also receives context such as saved TV names and smart channel names.
4. **Plan.** The planner turns the intent into a list of concrete steps. *"Volume to 20"* with an estimated volume of 50 becomes 30 volume-down presses with short delays, and then the estimate is updated to 20.
5. **Execute and confirm.** The executor sends the steps to the TV (or to the connection, settings or navigation parts of the app) and speaks a short confirmation using the phone's built-in text-to-speech.

### Hybrid understanding engine

| Pass | What it does | Why |
|---|---|---|
| **0. Learned corrections** | Looks up phrases the user has corrected before (up to 500, least recently used dropped first). | Adapts to each user's accent and wording. |
| **1–2. Rule engine** | Exact, then fuzzy, keyword matching across all alternative transcripts, in 18 languages, with channel name and number heuristics. | Instant, deterministic, and works on phones that cannot run the model. |
| **3. Local language model** | LFM2.5-350M reads the request and returns a structured intent through function calling. It is used only when the rules are not confident enough. It is also consulted when a lower-ranked transcript wins by a large margin. | Handles free-form and unusual phrasing. |

Results below a confidence threshold are not executed blindly. The assistant asks *"Did you mean…?"* or says it did not understand. A separate **multi-turn voice chat** mode keeps a conversation with the model and lets it call the same TV commands as tools. On iOS, system voice shortcuts can route directly into the same command path.

**Why LFM2.5-350M.** Understanding a TV command is a structured task (pick an intent from a closed list and fill in its details), not a knowledge task. The model is a small edge model trained for instruction following, data extraction and tool use. It is about 229 MB in its compressed form and is downloaded after install, so the app itself stays small. A typical intent takes well under a second on mainstream phones. If accuracy on the larger vocabulary is not good enough, the escalation path is a higher-precision variant, then few-shot examples in the prompt, then the larger LFM2.5-1.2B.

### Intent catalog

| Group | Intents | Example utterances |
|---|---|---|
| **Volume** | Volume up / down (with steps), set volume, mute, unmute, calibrate volume | "Turn it up", "Volume up 5 times", "Set volume to 30", "The volume is 35" |
| **Channels** | Change channel (number or name), next / previous, last channel, show channel list | "Channel 42", "Go to ESPN", "Go back to what I was watching" |
| **Playback** | Play, pause, stop, fast forward, rewind, record | "Resume", "Skip ahead 30 seconds" |
| **Power** | Power on, power off, sleep timer | "Turn off the TV" |
| **Navigation & keys** | Up / down / left / right / OK / back / home, exit, guide, info, tools, colour keys | "Press OK", "Open the guide", "Press red" |
| **Apps, input & subtitles** | Open app, input source, subtitles on / off, type text | "Open Netflix", "Switch to HDMI 2", "Enable captions" |
| **Connection** | Scan for TVs, connect, disconnect, turn on TV (network wake), switch TV | "Connect to my TV", "Switch to the living room TV" |
| **App navigation** | Go to screen (remote, TVs, settings, voice chat) | "Go to settings", "Show my TVs" |
| **Settings** | Change any setting, change app language | "Turn off haptic feedback", "Change the language to Turkish" |
| **Model management** | Download or delete the language model or the Whisper model | "Download the AI model", "Delete the Whisper model" |
| **Fallback** | Unknown | "Sorry, I didn't understand that." |

In total: 41 intents, all understood by the language model, handled by the planner and covered by the rule engine in at least English, Turkish and German.

### Zero-click flow

1. **First launch.** Onboarding offers the language model download (and optionally Whisper). The model comes from a public model host, with retries, a storage check and a Wi-Fi-only option.
2. **Connect by voice.** *"Connect to my TV"* scans the local network, connects and pairs, and opens the remote without any taps. *"Turn on the TV"* wakes a saved TV over the network and reconnects.
3. **Control by voice.** *"Switch the channel to Show TV"* matches the saved smart channel, dials its number and confirms. *"Mute and go to channel 5 then open the guide"* runs three steps in order.
4. **Configure by voice.** *"Set the wake word to hey remote"* or *"Change the language to Turkish"* asks for confirmation first, then applies the change.
5. **Move around by voice.** *"Go to settings"*, *"Show my TVs"* and *"Open the remote"* work from any screen. An optional setting (off by default) starts listening as soon as the app opens.

### Privacy and offline operation

- Speech recognition runs on the device, and no audio leaves the phone.
- Understanding and execution run on the device. No usage data or analytics are sent to any server.
- The network is needed only to download the models once. After that, everything works in airplane mode, except scanning for and connecting to TVs, which need a local network (Wi-Fi without internet is enough).
- Models are stored in the app's private storage and checked for integrity after download.
- On phones that cannot run the model (older Android versions, too little memory or an unsupported processor type), the rule engine keeps voice control working instead of the feature being switched off.

### Remote-control features alongside voice

- **TV manager:** online and saved TVs in one list, one-tap switching, pull to refresh, swipe to delete, pairing tokens kept across restarts.
- **Wake-on-LAN:** power on a saved TV from standby, then connect automatically once it responds.
- **Three control tabs:** *Standard* (classic remote with long-press digits that auto-confirm), *Smart* (pages of configurable channel buttons with names and logos, plus swipe gestures for volume and channel and double-tap to mute) and *Navigate* (a touchpad for pointer control, keyboard text input, and quick Home / Back / Search / Menu buttons).
- **Smart Source:** a grid of input sources with user-editable names such as "PlayStation" for HDMI 2.
- **Settings:** fixed-IP connection that skips network discovery, a VPN mode, remote layout choice, long-press delay, haptic feedback, language override, and resetting or exporting smart channels.
- **Reliability:** better detection of why a connection dropped (TV off or Wi-Fi lost) and automatic reconnection.
- **Localisation:** the interface is available in 18 languages.

## Implementation & Phasing

| Phase | Scope |
|---|---|
| **1. Remote foundation** | Saved TVs and pairing, TV manager, Wake-on-LAN, tabbed remote (Standard / Smart / Navigate), Smart Source, settings, manual IP and VPN mode, haptics, auto-reconnect, interface translations. |
| **2. On-device voice core** | Model download on first launch, speech-to-text, spoken confirmations, the hybrid engine with the local model, the original 21-intent catalog, device capability checks with a rule-based fallback, learned corrections, multi-turn voice chat, and iOS voice shortcuts. |
| **3. Voice recognition overhaul** | Tuned on-device dictation with alternative transcripts, native audio capture, voice-activity detection with pre-roll, a transcript-based wake phrase, optional offline Whisper, and a mic-level indicator with an optional debug view. |
| **4. Action planning core** | Separate understanding from execution: a deterministic planner and a step-by-step executor, with the existing commands rebuilt on top. |
| **5. Expanded intent catalog** | Grow from 21 to 41 intents covering connection, extra keys, app navigation, settings, model management and calibration. |
| **6. Settings by voice** | Every setting reachable by voice, with validation, spoken confirmation and a confirmation step for risky changes. |
| **7. Smarter understanding** | Compound commands, context (saved TV and channel names) passed to the model, and follow-up questions for confirmation or clarification. |
| **8. Zero-click everywhere** | Voice on every screen, voice-driven scan, connect and wake, and optional listening on app launch. |
| **9. State estimation** | Estimated volume and mute state with voice recalibration, and channel memory for "last channel". |
| **10. Model and speech polish** | More robust downloads (retries, storage check, Wi-Fi-only, mirror), model management by voice, an optional high-accuracy model variant, and command vocabulary hints for recognition. |
| **11. Validation** | Tests of the planner and executor, a corpus of reference utterances in several languages, and a manual test matrix on real Android and iOS phones with a real TV. |

The zero-click milestone is complete when all of these work on real phones with a real TV: connecting with no touches after onboarding, channel changes by name, target volume, compound commands, settings and navigation by voice, network wake, rule-based coverage on an unsupported phone, and airplane-mode operation after the download.

## Stakeholders & Benefits

| Stakeholder | Benefit |
|---|---|
| **TV viewers** | Natural-language control of the TV and the app, including compound and name-based requests, without looking for buttons. |
| **People who struggle with small buttons or with seeing them** | Every action, from connecting to the TV to changing settings, can be done by voice, with spoken confirmation of what happened. |
| **Privacy-conscious users** | No audio or usage data leaves the phone. The assistant works offline. |
| **Multilingual households** | The interface in 18 languages, rule tables in 18 languages, and settings words in English, Turkish and German. |
| **Users with older or lower-end phones** | The rule-based fallback keeps voice control available where the model cannot run. |
| **App publisher** | No server-side costs for voice, and a small app download because models are fetched after install. |
| **Open-source and edge-AI community** | A concrete reference for combining small local language models with deterministic rules and planning in a consumer app. |

## Risks & Mitigations

| Risk | Mitigation |
|---|---|
| **The model cannot run on a large share of phones** (roughly 35% of Android devices are below the required Android version, and some phones have too little memory) | Runtime capability check; the rule-based engine covers common commands on those phones. |
| **The model runs out of memory on a phone that looked capable** | Detect the failure, turn off the model for that install and fall back to the rules. |
| **Accuracy drops as the vocabulary grows** | Higher-precision model variant, few-shot examples, then a larger model. The planner, not the model, handles decomposition. |
| **Misheard or low-confidence commands** | Confidence thresholds, several alternative transcripts, learned user corrections, and spoken "Did you mean…?" confirmation. |
| **Risky changes made by mistake** (language switch, model deletion, wake phrase) | An explicit spoken confirmation step before applying them. |
| **The TV cannot report volume, channel or power state** | Estimated state, voice recalibration, and channel memory. The limitation is openly documented. |
| **Model download fails or is too large for the connection or device** | Retries with backoff, a mirror source, a free-space check, a Wi-Fi-only default, and clear spoken or on-screen errors. |
| **Third-party wrapper libraries for the model runtime become unmaintained** | Pinned versions and an interchangeable alternative wrapper. |
| **An always-on acoustic wake word could not be bundled** | A transcript-based wake phrase is used instead. An acoustic wake engine remains an optional next step. |
| **Model licence requires attribution** | Attribution in the app's About screen and store listings. |
| **Only Samsung TVs are supported in practice** | TV control sits behind a common interface. Support for another brand is currently a placeholder. |

## Acknowledgements

The basic remote-control foundation is based on an MIT-licensed open-source Samsung TV remote project by mazen-salah; the ideas described here are the additions on top of it.

## Origin

Originally designed by Merih İlgör (2026); published here as an open project idea.

## License

This document and the project idea it describes are licensed under CC BY-NC-SA 4.0 for non-commercial use (see [LICENSE](LICENSE)). Commercial use requires a revenue-share agreement (see [COMMERCIAL-LICENSE.md](COMMERCIAL-LICENSE.md)). Third-party components, including the MIT-licensed remote-control base and the language and speech models, remain under their own licenses.
