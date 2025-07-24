# Changelog

## 0.2.2 (2025-07-24)

Full Changelog: [v0.2.1...v0.2.2](https://github.com/thshao2/head-tracking-chrome-extension/compare/v0.2.1...v0.2.2)

### Features:
- When clicking on “Start Head Tracking”, the cursor is now initialized at the center of the screen. Previously, it was at the top left corner. 

### Bug Fixes:
- Fixed an intermittent bug where the area and locations the cursor was allowed to move in were completely off once “Start Head Tracking” was clicked. This occurred due to a race condition in which the calibration file was still being processed under the hood, but the MediaPipe FaceLandmarker pipeline was already completed and sending facial landmark data. This resulted in a fallback algorithm being used that would attempt to control the cursor through a basic, uncalibrated mapping. This basic, uncalibrated algorithm caused unexpected behaviors to the location of the cursor even once the calibration file was fully processed.

## 0.2.1 (2025-07-23)

Full Changelog: [v0.2.0...v0.2.1](https://github.com/thshao2/head-tracking-chrome-extension/compare/v0.2.0...v0.2.1)

### Bug Fixes:
- Fixed a UI bug where if the user selected “Raise Eyebrows” as a click action, an incorrect description was shown. The correct description of “Raise your eyebrows to click” is now shown correctly for this respective click action.
- Fixed a bug where if a user opted to stop the head tracking process while a webpage was still scrolling, the page would continue to actively scroll.
- Fixed a bug where starting the head tracking process a second time would not properly reinitialize the cursor on all active tabs (excluding the current focused tab).

## 0.2.0 (2025-07-22)

Full Changelog: [v0.1.0...v0.2.0](https://github.com/thshao2/head-tracking-chrome-extension/compare/v0.1.0...v0.2.0)

### Features:
- **Increased Streaming Rate:** The rate at which the cursor’s position is updated has been increased from 30fps (30hz) -> 60fps (60hz). This allows for a smoother experience from the user.

- **Camera:** Once the user has granted the chrome extension “Allow” permission to access the camera, future openings of the chrome extension popup will automatically enable the camera, saving the user one additional click in the setup phase. At this point, the only button the user will ever need to click to start the head-tracking process is the button “Start Head Tracking”.
- **Settings:** Update the status page during head-tracking to include a “Settings” section. The settings section allows for the user to configure the following during tracking:
  - The exponential smoothing factor. A slider will allow the user to choose a value between 0.5 -> 0.99 (0.95 is the default).
  - An option to choose a click action to perform a “left-click” on a webpage. By default, no option is selected. The user can choose between three options (for now): smile, raise their eyebrows, and open your mouth wide.
  - Click Assist (explained in further detail below): The user can turn it on/off (default is off).
- **Auto-Save:** All settings are automatically saved by the Chrome Extension.
- **Click Assist:** Performing facial gestures while keeping the cursor inside an interactive element can be difficult. “Click Assist” creates a 100px buffer that allows the cursor to still be highlighted green (indicating that a click can be performed) even after moving up to 100px away from the interactive element. Thus, once a cursor enters an interactive element, it will be locked to that element (even if it enters a new button < 100px away) to allow the user to click on that interactive element even if the cursor moves some px off of it. In order to have the cursor become “unclocked” from an element, the user must move > 100px, and then can lock on to another interactive element.
- **Click Action:** Different users might find different facial gestures easier to perform, or better situated to the “clicking” use case (e.g. to prevent accidental clicking). Thus, a new option allows users to choose out of 3 facial gestures: smiling, raising their eyebrows, and opening their mouth/jaw, in order to perform a left-click. If the user may never need to perform a left-click, this feature can be turned off by setting the click action to “None”.


### Bug Fixes:

- Update the Exponential Smoothing Factor to use a parameter of $0.95^k$, where k=2 to replicate the behavior observed in the original head tracking [website](https://head-control-website.vercel.app/).
- **Camera Permissions Access:** Fixed a bug where if the user had the camera permission setting to “Ask (Default)”, an error message would still be displayed, forcing the user to have to navigate to the chrome extension settings and change the camera setting from “Ask (Default)” -> “Allow”. Now, if the user has the camera permission set to “Ask (Default)”, a new options tab will open, prompting the user the option to click on “Allow while visiting the site” and “Allow this time”. Unfortunately, due to how chrome extensions work, the user must click on “Allow while visiting this site” and NOT “Allow this time” to grant the chrome extension persistent access to the camera. New error alerts and clearer instructions have been added to ensure the user will not run into any issues/confusion in the setup phase.

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