# TinyGS Web Installer

## Overview

The TinyGS Web Installer is a browser-based tool for flashing TinyGS firmware onto supported ESP32-based devices. It provides an easy-to-use interface for users to select and install firmware releases directly from the web, without needing additional desktop software. This project supports multiple ESP variants including ESP32, ESP32-C3, ESP32-S3, and others.

TinyGS is an open-source ground station network for LoRaWAN/IoT devices, and this installer simplifies the deployment of its firmware.

### Key Features

- Web-based flashing interface using Web Serial API.
- Support for multiple firmware releases stored in the `bin/` directory.
- Automatic device detection and chip variant selection.
- Progress tracking and error handling during installation.
- Deployed via Cloudflare Pages (see `.github/workflows/deploycf.yaml` for deployment automation).

## Prerequisites

- A modern web browser that supports the [Web Serial API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Serial_API) (e.g., Chrome 89+, Edge 89+, or Opera 76+).
- A compatible ESP32 development board connected via USB.
- USB drivers for your ESP32 board (e.g., CP210x or CH340 drivers, depending on the board).
- Enable USB debugging and serial access in your browser (e.g., chrome://flags/#enable-experimental-web-platform-features if needed).

## Quick Start

1. **Access the Installer**:
   - Clone or download this repository.
   - Open `index.html` in a compatible browser, or deploy to a web server (recommended for production).

2. **Connect Your Device**:
   - Plug in your ESP32 board via USB.
   - Click the "Connect" button in the installer to grant serial port access.

3. **Select Firmware**:
   - Choose the appropriate release from the `bin/` directory (e.g., `release_2509061` for the latest).
   - Select your chip variant (ESP32, ESP32-C3, etc.).

4. **Flash the Firmware**:
   - Click "Install" to begin the flashing process.
   - Monitor the progress in the interface. The tool will handle erasing, writing, and verifying the firmware.

5. **Verify Installation**:
   - Once complete, disconnect and reconnect the device to boot the new firmware.
   - Use a serial monitor (e.g., Arduino IDE Serial Monitor at 115200 baud) to check for TinyGS boot messages.

## Usage Details

### Installer Components

- **index.html**: Main entry point with UI for device connection and firmware selection.
- **script.js**: Handles Web Serial API integration, file loading, and flashing logic.
- **style.css**: Styles for the user interface.
- **installer/**: Directory containing modular JS for chip-specific installation logic:
  - `esp32-*.js`, `esp32c3-*.js`, etc.: Variant-specific flashing scripts.
  - `install-button.js`: UI component for the install action.
  - `rom-*.js`: Handles ROM detection and bootloader integration.
- **bin/**: Firmware release directories:
  - Each release (e.g., `release_2509061`) contains `manifest.json` with metadata and `.bin` files for each chip variant (e.g., `esp32_2509061_merged.bin`).

### Custom Firmware

To add a new release:

1. Place your merged `.bin` files and `manifest.json` in a new subdirectory under `bin/` (e.g., `bin/release_newversion/`).
2. Update `script.js` or the manifest loader to include the new release.
3. Commit and deploy.

### Troubleshooting

- **Permission Denied**: Ensure the browser has access to serial ports. Restart the browser if needed.
- **Device Not Detected**: Check USB connection, drivers, and try a different cable/port.
- **Flashing Fails**: Verify the correct chip variant and firmware compatibility. Check browser console for errors.
- **Slow Performance**: Web flashing can be slower than desktop tools; consider using esptool.py for bulk operations.
- For advanced debugging, refer to the TinyGS documentation: [TinyGS GitHub](https://github.com/tinygs/tinyGS).

## Building and Deployment

### Local Development

1. Clone the repository:

   ```
   git clone <repo-url>
   cd tinygs_web_installer
   ```

2. Open `index.html` in a local server (e.g., using Python: `python -m http.server 8000`) to avoid CORS issues with file loading.
3. Edit files as needed and test flashing.

### Deployment to Cloudflare Pages

- This project uses GitHub Actions for automated deployment (see [`deploycf.yaml`](.github/workflows/deploycf.yaml)).
- Deployment is triggered by publishing a new GitHub release (automatic) or manually via the "Run workflow" button in the GitHub Actions tab (workflow_dispatch).
- Ensure your Cloudflare Pages project is connected to this GitHub repository.
- Access the live site at your Cloudflare Pages URL.

### Dependencies

- No external Node.js dependencies; pure frontend with vanilla JS.
- Relies on browser APIs; no build step required.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

*Last updated: September 2025*
