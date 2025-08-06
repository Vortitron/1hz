# 1hz
# EarthSync Web App

## Overview
EarthSync is a web-based application designed to provide users with real-time location-based information related to Earth's rotation. The app leverages the browser's Geolocation API to determine the user's latitude, longitude, and speed, calculates the Earth's rotational distance at their latitude, and adjusts a trigger interval based on their longitudinal movement relative to Earth's rotation. A visual indicator flashes at the adjusted interval, and users can toggle between two modes to explore different behaviors. The app is designed to run on mobile devices, making it accessible via a web browser on smartphones.

This project aims to create an engaging and educational tool that visualizes how movement affects perceived time relative to Earth's rotation. A future extension will incorporate a YouTube video player with playback speed adjusted dynamically based on the user's movement-adjusted interval, adding a fun and interactive element.

## Features
### Current Features
1. **Real-Time Location Display**:
   - Shows the user's current latitude and longitude with high accuracy using the browser's Geolocation API.
   - Updates dynamically as the user moves.
   - Displays an error message if geolocation is unavailable or denied.

2. **Speed Tracking**:
   - Displays the user's current speed (in meters per second and kilometers per hour) based on GPS data.
   - Tracks longitudinal speed (eastward or westward movement) for use in calculations.

3. **Earth's Rotational Distance**:
   - Calculates the Earth's circumference at the user's latitude using the oblate spheroid model.
   - Computes the distance traveled in one second due to Earth's rotation (e.g., ~258.18 m/s at latitude 56.3708°).
   - Displays the result in meters and kilometers.

4. **Movement-Adjusted Trigger Interval**:
   - Calculates the adjusted trigger interval based on the user's longitudinal speed relative to Earth's rotational speed.
   - In **Mode 1**, triggers an action every second, corresponding to the Earth's rotational distance at the user's latitude.
   - In **Mode 2**, adjusts the trigger interval:
     - Stationary: Triggers every 1 second.
     - Eastward at Earth's rotational speed (e.g., ~929 km/h at 56.3708°): No triggers (interval approaches infinity).
     - Westward at Earth's rotational speed: Triggers every ~0.5 seconds.
   - Displays the difference between the user's speed and Earth's rotational speed.

5. **Visual Indicator**:
   - A visual element (e.g., a flashing circle or text) activates at the calculated trigger interval.
   - The flash reflects the mode: fixed 1-second intervals in Mode 1, or adjusted intervals in Mode 2.

6. **Mode Toggle**:
   - A toggle button allows users to switch between Mode 1 (fixed rotational distance) and Mode 2 (movement-adjusted interval).
   - The interface clearly indicates the current mode.

### Planned Features
1. **YouTube Video Playback with Dynamic Speed**:
   - Integrate a YouTube video player (using the YouTube IFrame Player API).
   - Adjust the video playback speed based on the movement-adjusted trigger interval from Mode 2.
     - Stationary: Normal playback speed (1x).
     - Eastward movement: Slower playback (approaching 0x if matching Earth's rotational speed).
     - Westward movement: Faster playback (up to 2x if matching Earth's rotational speed in the opposite direction).
   - Allow users to select or input a YouTube video URL for playback.
   - Add a fun, interactive element to make the app engaging (e.g., syncing video speed to "feel" the Earth's rotation).

2. **Enhanced UI/UX**:
   - Add animations for the flashing indicator to improve visual appeal.
   - Include a map (e.g., using Leaflet or Google Maps API) to visualize the user's location.
   - Provide a settings panel to customize the flashing indicator (color, style) or select predefined YouTube videos.

3. **Data Logging**:
   - Log location, speed, and trigger interval data for analysis or visualization.
   - Allow users to export data for educational or experimental purposes.

## How It Works
- **Geolocation**: The app uses the browser's `navigator.geolocation` API to obtain high-accuracy latitude, longitude, and speed data. Updates are requested continuously to track movement.
- **Earth’s Rotation Calculation**:
  - Computes the radius of the parallel of latitude using the formula:  
    \[
    R_{\text{parallel}} = \frac{a \cos\phi}{\sqrt{1 - e^2 \sin^2\phi}}
    \]
    where \( a = 6,378,137 \, \text{m} \) (equatorial radius), \( e^2 \) is the eccentricity squared, and \( \phi \) is the latitude in radians.
  - Calculates circumference (\( 2 \pi R_{\text{parallel}} \)) and rotational distance per second (\( \text{circumference} / 86,164 \), using a sidereal day).
- **Mode 1**: Triggers a visual flash every second, representing the fixed rotational distance (e.g., 258.18 meters at 56.3708°).
- **Mode 2**: Adjusts the trigger interval based on the user’s longitudinal speed:
  \[
  T = \frac{v_{\text{Earth}}}{v_{\text{Earth}} - v_{\text{longitudinal}}} \cdot 1 \, \text{second}
  \]
  where \( v_{\text{Earth}} \) is the Earth’s rotational speed at the latitude, and \( v_{\text{longitudinal}} \) is the user’s eastward (positive) or westward (negative) speed.
- **Visual Feedback**: A DOM element (e.g., a div or SVG) flashes at the calculated interval, providing immediate feedback.
- **Mode Toggle**: A button toggles between modes, updating the trigger logic and UI accordingly.

## Use Cases
- **Educational Tool**: Teaches users about Earth’s rotation, latitude-dependent rotational speed, and relative motion.
- **Interactive Experiment**: Demonstrates how movement affects perceived time relative to Earth’s rotation.
- **Fun Application**: The YouTube video playback feature will make the app entertaining, allowing users to “feel” their motion through video speed changes.
- **Mobile Accessibility**: Runs on smartphones via a web browser, making it widely accessible without requiring app installation.

## Technical Details
- **Platform**: Web-based, optimized for mobile browsers (Chrome, Safari, Firefox).
- **Technologies**:
  - HTML5, CSS3, JavaScript (ES6+).
  - Geolocation API for location and speed data.
  - Future: YouTube IFrame Player API for video playback, potentially Leaflet for maps.
- **Dependencies**: None initially; YouTube API and possibly mapping libraries for future features.
- **Browser Compatibility**: Requires a modern browser with Geolocation API support and GPS access (most smartphones).
- **Accuracy**: Depends on the device’s GPS hardware (typically 2.5–5 meters for high-accuracy mode).
- **Limitations**:
  - Requires GPS signal (works best outdoors, may struggle indoors).
  - Speed calculations may be noisy; averaging may be needed for Mode 2.
  - YouTube playback requires internet access and may be subject to API quotas.

## Installation
1. Clone the repository:
   ```bash
   git clone https://github.com/[your-username]/EarthSync.git
   ```
2. Host the web app on a local or remote server (e.g., using `http-server` or a static hosting service like GitHub Pages).
3. Open the app in a mobile browser with GPS enabled.
4. Grant location permissions when prompted.

## Usage
1. Open the web app on your smartphone.
2. Allow geolocation access to display your latitude, longitude, and speed.
3. View the Earth’s rotational distance per second at your latitude and the difference based on your speed.
4. Observe the flashing indicator, which triggers every second (Mode 1) or at the adjusted interval (Mode 2).
5. Use the toggle button to switch between modes.
6. (Future) Select a YouTube video to play with speed adjusted based on your movement.

## Future Development
- **YouTube Integration**: Implement dynamic video playback speed adjustment using the YouTube IFrame Player API.
- **UI Enhancements**: Add a map, customizable flashing indicators, and a settings panel.
- **Data Visualization**: Include graphs or logs of speed and trigger intervals over time.
- **Offline Support**: Cache last-known location for limited functionality without GPS.
- **Cross-Platform Expansion**: Adapt the logic for an ESP32-based device for hardware enthusiasts.

## Contributing
Contributions are welcome! Please:
1. Fork the repository.
2. Create a feature branch (`git checkout -b feature/your-feature`).
3. Commit changes (`git commit -m "Add your feature"`).
4. Push to the branch (`git push origin feature/your-feature`).
5. Open a pull request.

## License
This project is licensed under the MIT License. See the `LICENSE` file for details.

## Contact
For questions or suggestions, open an issue or contact [your-username] on GitHub.

---

### Notes
- This README assumes a GitHub repository structure and includes placeholders (e.g., `[your-username]`) that you can replace with your actual GitHub username.
- It outlines the current functionality based on your requirements and sets clear expectations for the YouTube video feature as a planned extension.
- The tone is professional yet accessible, suitable for developers and users interested in the project.
- If you want to adjust the scope, add specific styling guidelines (e.g., for the flashing indicator), or include additional details (e.g., specific YouTube video ideas), let me know, and I can refine the README.
- The next step would be to provide the HTML/CSS/JavaScript code for the web app, which I can do if you’re ready. Just confirm, and I’ll include the full implementation with comments.

Let me know if you want to tweak the README or proceed with the code!
