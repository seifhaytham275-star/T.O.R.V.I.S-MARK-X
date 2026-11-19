# T.O.R.V.I.S — Upgraded Edition

Torvis is a local-first desktop AI assistant. This upgraded package preserves the existing dashboard, Gemini chat, voice input/output, screen visualisation, system metrics, app launching, file tools, routines/protocols, and phone remote.

## What's improved

- **Safer upgrade defaults:** the first run of the upgraded version migrates the config to schema v2 and switches Computer Control and Full Control OFF. Re-enable only the permissions you actually need in Settings.
- **Safer browser startup:** the launcher no longer uses Chromium's fake microphone-permission flag. The browser asks for normal microphone permission.
- **Safer HTML rendering:** task labels, status values, and model names are inserted with DOM `textContent`/`Option` nodes instead of interpolated HTML.
- **HTTP hardening:** security response headers, stricter Host/Origin parsing, token protection for the phone page, bounded JSON request sizes, bounded phone command queue, and bounded Gemini proxy payloads.
- **More reliable files and screenshots:** atomic config writes, validated screenshot dimensions/quality, and collision-resistant screenshot names.
- **Performance/accessibility:** canvas device-pixel-ratio cap, reduced-motion support, and microphone auto-start OFF unless explicitly enabled.
- **Project consistency:** `torvis.html` is synchronised with the actual embedded dashboard in `main.py`; `torvis.py` is included as a compatibility launcher.

## Requirements

- Python 3.10 or newer is recommended.
- Windows is the primary target in the supplied launcher; macOS/Linux paths are retained where the original code supports them.
- A Gemini API key is required for Gemini features.
- Microphone access requires a supported browser and the user's permission.
- Some features are optional and depend on installed packages. Torvis shows missing capabilities in Settings.

## Install and run (Windows)

1. Extract the ZIP to a folder you control.
2. Open Command Prompt or PowerShell in that folder.
3. Create a virtual environment:

   ```powershell
   py -m venv .venv
   .venv\Scripts\activate
   ```

4. Install dependencies:

   ```powershell
   python -m pip install --upgrade pip
   pip install -r requirements.txt
   ```

5. Start Torvis:

   ```powershell
   python main.py
   ```

   Or use `python torvis.py` as the compatibility launcher.

6. Add your Gemini API key in the sign-in dialog. Keep it private.

## First-run permissions

The upgraded config migration deliberately switches both **CONTROL** and **FULL CONTROL** off once. Open Settings and enable only what you need.

- **CONTROL** allows supported mouse/keyboard, screen, clipboard, and app actions.
- **FULL CONTROL** unlocks local file operations and shell commands. Leave it OFF unless you genuinely need it.
- Keep **ASK BEFORE COMMANDS** ON. A command approval prompt is an important safety boundary.
- Microphone auto-start is OFF by default. Turn it on only if you want that behaviour.

The app's phone remote uses an HTTP connection over your local network and a long random token. Use it only on a trusted private Wi-Fi network, never on public Wi-Fi. Rotate the phone code if a QR code or link is shared accidentally. The server listens on the LAN so the phone can reach it; do not expose port 8123 to the public internet or configure router port-forwarding.

The Gemini key is still saved in the browser's local storage by the existing application design. Anyone with access to the same browser profile or Windows account may be able to access it. Use a dedicated Windows account and do not share the profile. For a production deployment, move key storage to an OS credential vault.

## Main project files

| File | Purpose |
|---|---|
| `main.py` | Main Python launcher, local HTTP server, computer actions, metrics, embedded dashboard and remote page |
| `torvis.html` | Standalone copy of the main dashboard; kept in sync with `main.py` |
| `torvis.py` | Compatibility launcher that calls `main.main()` |
| `requirements.txt` | Python dependencies for core features and optional plugins |
| `tests/test_safety.py` | Basic regression tests for URL/path restrictions and disabled-control behaviour |
| `Torvis_Explained.pptx` | Slide presentation explaining architecture, features, workflows and upgrade changes |
| `Presentation_Script.md` | Suggested speaker notes for the slide deck |

## Run the tests

```powershell
python -m unittest discover -s tests -v
```

The tests do not need a Gemini key or an active Torvis server.

## Important limitations

This is a careful upgrade of the supplied code, not a full security certification. The original app can control the desktop and execute local shell commands when explicitly enabled. Review every permission before enabling it. The local HTTP remote is intended for a trusted LAN, not the public internet.
