# LinguaDragon Local 🐉

Live Japanese and English captions with Chinese translation, contextual reply suggestions, and phone viewing — built for macOS.

**Version:** 0.9  
**Platform:** Apple Silicon · macOS 26 or later  
**App interface:** Chinese

## Overview

LinguaDragon Local helps you follow meetings, interviews, lectures, and videos in Japanese or English.

Speech recognition and translation run locally on your Mac after the required language packs are downloaded. Optional Codex integration provides contextual replies based on the conversation and your reference documents.

## Features

### Live transcription and translation

- Transcribe Japanese or English and translate it into Chinese.
- Show recognition and translation previews while the speaker is still talking.
- Capture system audio from a selected application or screen.
- Listen through the built-in microphone or an external microphone.
- Display Chinese captions in a floating desktop window.

### Context-aware replies

- Select a caption and ask Codex for a suggested response.
- Include up to five preceding caption entries to provide context.
- Combine related questions and interpret ambiguous transcription using the surrounding conversation.
- Display Chinese and Japanese replies with Japanese reading annotations.
- Type your own Chinese response and translate it into Japanese.
- Replies are never automatically sent or spoken.

### Reference documents

- Attach a folder containing prepared notes, papers, or interview materials.
- Support text-based PDF, DOCX, Markdown, and UTF-8 TXT.
- Retrieve relevant passages when requesting a Codex reply.
- Save the reference-folder association and your background information with a meeting project.

### Phone viewing

- Scan a QR code to open the companion page in your phone's browser.
- View live recognition and Chinese translation previews.
- Select finalized captions and request replies from your phone.
- Enter Chinese text and receive Japanese translation with reading annotations.
- No APK or separate mobile app is required.

### Meeting records

- Create named meeting projects.
- Save transcripts locally.
- Export a transcript PDF manually or automatically on normal app exit.

## Requirements

| Component | Requirement |
|---|---|
| Mac | Apple Silicon |
| Operating system | macOS 26 or later |
| Local captions | Downloaded speech-recognition and translation language packs |
| Microphone mode | macOS microphone permission |
| System audio mode | macOS capture authorization and source selection |
| Phone viewing | Mac and phone on the same trusted local network |
| Optional Codex replies | Compatible Codex CLI, your own ChatGPT-based Codex login, internet access, and available usage allowance |
| Optional local reply model | Apple Intelligence and an available system language model |

The included application is built for **arm64** and does not support Intel Macs.

Basic local transcription and translation do not require an OpenAI API key or Codex login. Optional Codex replies use the signed-in account's allowance.

## Download and Installation

1. Download `LinguaDragonLocal-0.9-GitHub.zip` from this repository.
2. Extract the archive.
3. Move `LinguaDragonLocal.app` to your Applications folder.
4. Open the app.
5. Open **Offline Settings (`离线设置`)** and prepare the required language packs.
6. Select Japanese or English and choose an audio source.
7. Click **Start Translation (`开始翻译`)**.

The included app is ad-hoc signed and **not notarized by Apple**. macOS may block a downloaded copy. If you trust the source, review the available option in System Settings → Privacy & Security, or build the app yourself.

## Choosing an Audio Source

### System audio

Use this mode for meetings or videos playing on your Mac.

When prompted, select the application or screen containing the audio you want to capture.

### Microphone

Use this mode to listen to nearby speakers or an external audio source.

For an external microphone:

1. Connect the microphone.
2. Select it in **System Settings → Sound → Input**.
3. Choose microphone mode in LinguaDragon Local.
4. Start translation and grant microphone access if requested.

Stop translation before changing the audio source or input device.

The app does not mix system audio and microphone input simultaneously or automatically distinguish speakers.

## Using Codex Replies

1. Set up a compatible Codex CLI and sign in using your own ChatGPT account.
2. Select a finalized caption in the app or on the phone page.
3. Choose Codex and optionally enter a specific question.
4. Enable reference documents if you want them included.
5. Request a reply.

The app sends the selected caption, up to five preceding caption entries, your question and background, and any retrieved reference excerpts.

It does not automatically access hidden memories from other Codex conversations.

Results include Chinese and Japanese suggestions. Japanese reading annotations are generated locally and may contain errors, especially for names and specialist terminology.

## Using Reference Materials

1. Place your materials in a dedicated folder.
2. Create or open a meeting project.
3. Click **Select Reference Materials (`选择参考资料`)**.
4. Describe your role or background where appropriate.
5. Keep document references enabled when requesting a reply.

Supported formats:

- PDF with an extractable text layer
- Word `.docx`
- Markdown `.md` and `.markdown`
- UTF-8 `.txt`

Scanned PDFs require OCR before use. PowerPoint files are not directly supported; export them as text-based PDFs first.

Retrieval uses keyword matching and bounded text extraction. It may miss relevant passages, and large documents can take longer to process. Only selected excerpts are sent with a Codex request, rather than the entire folder.

The project stores the folder path. If you move the folder, select it again.

## Connecting Your Phone

1. Connect your Mac and phone to the same trusted Wi-Fi network.
2. Open **Phone Display (`手机显示`)** on the Mac.
3. Scan the QR code and open the link in your phone's browser.
4. Keep the Mac awake and the app running.

The phone page displays incremental recognition and translation previews, recent captions, and reply controls. It polls for updates every 250 ms after each completed request; actual latency depends on processing and network conditions.

Stopping phone sharing invalidates the pairing link. Scan the new QR code after starting a new session.

The connection uses token-protected, **unencrypted local HTTP**. Keep the QR code private and use a trusted network. Guest Wi-Fi isolation or firewall rules may prevent devices from connecting.

Phone reply requests do not open the desktop reply panel. However, whole-screen sharing may still reveal application windows, system prompts, or notifications.

## Saving Meeting Records

Create a project to save the meeting transcript.

Project files are stored under:

```text
~/Desktop/会议记录/
```

The app saves project data locally and exports a PDF on normal exit. You can also export a PDF manually.

A forced quit or power interruption cannot trigger the exit-time PDF export. Saved project data can be reopened later.

## Privacy

- Speech recognition and translation run locally after language-pack setup.
- The app does not store captured audio or screen video.
- Optional Codex requests send the selected text, preceding context, and enabled document excerpts to the service.
- The distribution package contains no personal meeting records, reference documents, API keys, or login credentials.
- Each user should sign in with their own Codex account.

## Build from Source

Extract the distribution archive and open a terminal in its project directory.

Run:

```sh
zsh build.command
```

Building requires Swift, Clang, and an Apple SDK with the macOS 26 APIs used by the project.

The build script:

1. Compiles the bundled QR-code implementation.
2. Compiles the Swift sources for Apple Silicon.
3. Replaces the executable inside `LinguaDragonLocal.app`.
4. Applies an ad-hoc signature.

Keep the included app bundle in place because it supplies the resources, icon, and `Info.plist`.

## Package Structure

```text
LinguaDragonLocal-0.9/
├── README.md
├── .gitignore
├── build.command
├── Source/
├── Vendor/
├── THIRD_PARTY_NOTICES.txt
└── LinguaDragonLocal.app/
```

## Limitations

- Recognition and translation accuracy depend on speech clarity, terminology, and local model performance.
- Live previews may change as recognition is corrected.
- Response time depends on device resources, network conditions, and Codex processing.
- Reference retrieval is not a complete analysis of every document.
- Japanese reading annotations may be incorrect.
- The current interface is primarily Chinese.
- The included binary is not Apple-notarized.

## License and Attribution

The bundled QR-code library by Project Nayuki retains its MIT license notice. See `THIRD_PARTY_NOTICES.txt`.

No license has yet been selected for the original application code or artwork.

LinguaDragon Local is an independent project and is not an official OpenAI product.
