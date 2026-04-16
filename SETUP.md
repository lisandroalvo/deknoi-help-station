# Help Station — Setup Guide

## Quick start

1. Open `index.html` in a browser (Chrome recommended for kiosk mode)
2. Edit the `CONFIG` object at the top of the `<script>` section in `index.html`

---

## Configuration (inside index.html)

```js
const CONFIG = {
  whatsappNumber:   "66812345678",         // Your number, no + or spaces
  phoneNumber:      "tel:+66812345678",    // Your number with tel: prefix
  pdfUrl:           "guide.pdf",           // Path to your PDF guide
  whatsappMessage:  "Hi, I need help at the self-checkout station. 🛒",

  idleTimeout:      60,   // Seconds before showing "still there?" overlay
  warningCountdown: 10,   // Seconds to count down before resetting to idle
};
```

---

## Character image

Drop your character image file into this folder as:

```
character.png
```

Or edit the `<img src="character.png">` in the HTML to point to your filename.

**To switch to a looping video later**, replace the `<img>` tag with:

```html
<video
  id="character-img"
  src="character.mp4"
  autoplay loop muted playsinline
></video>
```

The CSS class `character-wrapper` handles sizing — no redesign needed.

---

## QR Code

The current QR is a visual placeholder.

To add a real QR code:
1. Generate a QR code image pointing to your WhatsApp link:
   `https://wa.me/YOUR_NUMBER?text=Hi%2C+I+need+help`
2. Save it as `qr-support.png` in this folder
3. Replace the `<canvas id="qr-canvas">` element with:
   ```html
   <img src="qr-support.png" alt="QR code — scan to contact support" style="width:100%;height:100%;object-fit:contain">
   ```

---

## PDF Guide

Place your PDF as `guide.pdf` in this folder,  
or change `pdfUrl` in CONFIG to an external URL.

---

## Running in Kiosk Mode (Chrome)

```bash
# macOS
/Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome \
  --kiosk \
  --no-first-run \
  --disable-pinch \
  --overscroll-history-navigation=0 \
  file:///path/to/HelpStation/index.html

# Windows
chrome.exe --kiosk --no-first-run "C:\HelpStation\index.html"
```

For a dedicated kiosk device, also add:
```
--disable-context-menu
--disable-infobars
--disable-session-crashed-bubble
```

---

## File structure

```
HelpStation/
├── index.html        ← entire app (single file)
├── character.png     ← your character image (drop here)
├── qr-support.png    ← your QR code image (drop here)
├── guide.pdf         ← your help PDF (drop here)
└── SETUP.md          ← this file
```

---

## Screens overview

| Screen       | Trigger                         |
|--------------|----------------------------------|
| Idle         | On load, after inactivity reset  |
| Home         | Tap idle screen                  |
| Guide        | Tap any guide card               |
| Sub-guide    | Tap sub-option in Product screen |
| Support      | Tap "Help Now" anywhere          |
| PDF          | Tap "Full guide" card            |

---

## Inactivity flow

1. User is on any non-idle screen
2. No interaction for `idleTimeout` seconds (default: 60s)
3. Overlay appears with countdown (`warningCountdown` seconds, default: 10s)
4. If no tap → app resets to idle screen
5. If user taps "I'm still here" → timer resets
