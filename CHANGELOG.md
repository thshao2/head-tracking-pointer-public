# Changelog

## 0.1.0 (2025-07-18)

### Features:
- **Model:** MediaPipe [FaceLandmarker](https://ai.google.dev/edge/mediapipe/solutions/vision/face_landmarker) to collect landmark data and capture facial expressions.
- **On Installation:** Users can upload their .csv configuration file to the popup, and click on “Enable Camera”. If the user does not have a file yet, a link that goes to https://head-control-website.vercel.app/ will allow the user to get the configuration file. The file uploaded will be saved permanently so that the user will not have to re-upload the file after turning off their computer or closing Chrome, while allowing for the user to change the file if needed. In order for “Enable Camera” to be activated, users must go to “Details -> Site Settings” in the extension page and set “Camera -> Allow”. Keeping camera permissions at “Ask (Default)” will not work. A preview of the camera will be shown, and users will be able to click on “Start Head Tracking”.
- **Head-Tracking:** During the head-tracking process, a custom cursor will show up on the user’s current webpage, moving the cursor based on the user’s head movements. The user can click on “Stop Head Tracking” at any time to remove the cursor from the webpage and stop the tracking process. The cursor will be updated at a rate of 30fps (30hz).
- **Click Action:** Raising your eyebrows up (sensitivity of 0.2) will trigger a “click” on the webpage.
- **Cursor:** The cursor will be highlighted green every time it hovers over a link (indicating a clickable).
- **Hands-Free Scrolling:** Moving the cursor down to the bottom or top of the screen and holding it for one second will begin a smooth scroll down or up, respectively. This only works for webpages and does not work for pdfs, Google Docs, etc.
Tracking Configuration: The current system is default to a 2D coordinate system, 3 landmark points, and an exponential smoothing factor of 0.95 (not configurable as of now).
- Opening a new tab and going to a new website will create a new custom cursor for each new webpage opened.
- Navigating between current active tabs (that were open before clicking on “Start Head Tracking”) should also show the currently active custom cursor on each tab, although at this time it is still a little buggy.
- _Limitations_: The cursor is unable to move up to the tabstrip, or access certain browser UI elements in Chrome.