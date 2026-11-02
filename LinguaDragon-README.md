# LinguaDragon Local

A native macOS companion for live Japanese/English-to-Chinese captions, optional contextual replies, and phone viewing over your local network.

**Version:** 0.9 · **Interface:** Chinese · **Platform:** Apple Silicon Mac, macOS 26 or later.

## Features

- Listen to system audio from a selected screen/application, or use the Mac's default microphone input, including an external microphone.
- Show incremental transcription and Chinese translation previews before the speaker finishes a sentence.
- Display a floating desktop caption window.
- Pair a phone by QR code; no Android APK is needed. The browser polls every 250 ms after each completed request and shows live previews, recent captions, and reply controls. Actual latency depends on the Mac and network.
- Ask Codex about a selected caption with up to five preceding caption entries. Related questions are answered together, with reasonable assumptions when transcription is ambiguous.
- Generate Chinese and Japanese reply suggestions with Japanese reading annotations. Suggestions are never automatically sent or spoken.
- Type a Chinese reply on your phone and translate it into Japanese with reading annotations on the Mac.
- Attach a reference folder to a meeting project and retrieve relevant document passages for replies.
- Save meeting projects locally and export a transcript PDF on normal quit or manually.

## Requirements

- Apple Silicon (arm64); the included binary does not support Intel Macs.
- macOS 26 or later. Development requires an Apple SDK that includes the macOS 26 Speech, Translation, and Foundation Models APIs, plus Swift and Clang.
- Download the selected speech-recognition and translation language packs in the app before first use. Initial downloads require internet access.
- Optional local reply generation requires Apple Intelligence and an available system language model. Availability depends on device, language, and system settings; it is separate from local transcription and translation.
- Optional Codex replies require a supported Codex CLI installation, the recipient's own ChatGPT-based Codex login, network access, and available account usage. No API key is required by this integration. No account credentials are included.

## Run the included app

1. Extract this archive on a compatible Mac.
2. Move `LinguaDragonLocal.app` to Applications or another preferred location and open it. You can drag it to the Dock.
3. This build is locally ad-hoc signed, **not Apple-notarized**. macOS may block a downloaded copy. If you trust the source, review the macOS Privacy & Security prompt; alternatively build it yourself. Do not disable system security globally.
4. Open the app's offline settings (`离线设置`) and prepare the language packs.
5. Select Japanese or English and choose the audio source (`声音来源`).
6. Click Start Translation (`开始翻译`) and approve the relevant macOS permissions. System audio uses a system sharing picker; microphone mode requires microphone permission.

For an external microphone, select it in **System Settings → Sound → Input** before starting. Stop translation before changing sources or devices. The app does not mix both sources or automatically identify speakers.

## Optional Codex replies

The app searches for the CLI inside supported ChatGPT/Codex app locations and `/opt/homebrew/bin/codex` or `/usr/local/bin/codex` (see `Source/CodexBridge.swift`). If needed, install a supported CLI and run `codex login` using your own ChatGPT account. The app checks login status before invoking it. CLI compatibility and account availability may change.

Select a finalized caption, choose Codex, and ask for a reply. The current caption, preceding context, your question/background, and enabled reference excerpts are sent to Codex. This uses your account allowance; basic local captions do not require Codex.

Japanese annotations are generated locally and may misread names or specialist terms. Responses and transcriptions can be incorrect; review important details before using them.

## Reference documents

Use a dedicated folder containing text-based PDF, DOCX, Markdown, or UTF-8 TXT files. PPT/PPTX and scanned-image PDF extraction are not supported; convert slides to a text-based PDF first. OCR is not included.

Create/open a meeting project, choose a reference folder, and describe your role/background. Folder associations are stored as paths, not copied documents. Reselect a folder after moving it.

Retrieval is keyword-based, not a complete reading of every document. Only selected excerpts are sent when you ask Codex. Current safeguards include up to 100 files, a 256 MB PDF limit, 20 MB for other files, up to 2,000 PDF pages, and bounded passage extraction. Each request uses up to 12 retrieved excerpts. Large documents may take time and relevant passages may be missed.

## Phone viewing

1. Connect the Mac and phone to the same trusted local network.
2. In the Mac app, open Phone Display (`手机显示`) and scan the QR code in a browser.
3. View live recognition and translated previews; select a finalized caption to request a reply.
4. Keep the Mac awake and the app running. After stopping/restarting phone sharing, scan the new code.

The service uses token-protected **unencrypted local HTTP**. Treat the QR code as access credentials; do not share it or use untrusted networks. Paired phones share the same reply result. Guest Wi-Fi isolation or firewall rules may prevent connection. The browser shows the most recent 60 caption entries; complete saved project records remain on the Mac.

Phone reply actions do not open the desktop reply sheet. This is not a guarantee that the app or macOS prompts will be invisible during whole-screen sharing; share the intended presentation window when appropriate.

## Meeting files and privacy

Projects are saved under `~/Desktop/会议记录/`. Normal quit exports a PDF; forced termination cannot run that export. Without a project, meeting records are not saved by the app. Audio and screen video are not stored by the app.

Local captioning and translation run on the Mac after language-pack setup. Optional Codex requests send the text described above to the service. The package contains no meeting transcripts, personal reference documents, API keys, or login credentials.

## Build from source

From the extracted project directory:

```sh
zsh build.command
```

The script compiles the vendored QR implementation, compiles `Source/*.swift` for arm64 macOS 26, replaces the executable inside the included app bundle, and ad-hoc signs that bundle. Keep the app bundle present: it provides the icon, resources, and Info.plist. Build artifacts go to `.build/`.

## Repository layout

- `Source/`: Swift app, audio capture, translation, project storage, Codex bridge, phone server/web UI.
- `Vendor/`: C QR-code implementation and wrapper.
- `LinguaDragonLocal.app/`: runnable macOS bundle and resources.
- `build.command`: local build script.
- `THIRD_PARTY_NOTICES.txt`: bundled QR-library license notice.

## Publishing to GitHub

Suggested repository name: `linguadragon-local`.

Extract the archive first, then upload/commit its contents to your repository so that this README and source files are directly visible. You can also attach the original ZIP as a downloadable Release asset. Creating a repository alone does not upload these files.

This package does not choose a license for your original code or artwork. Select an appropriate license before advertising the project as open source, and ensure you have permission to distribute the included artwork. The third-party QR library retains its MIT notice. This is an independent project, not an official OpenAI product.
