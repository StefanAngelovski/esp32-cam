# ESP32-CAM Web Server with Live Streaming

A simple ESP32-CAM web server that provides live MJPEG video streaming over WiFi using ESP-IDF.

## Features

- WiFi connectivity (STA mode)
- HTTP web server running on port 80
- Live MJPEG video streaming from the camera
- Simple HTML interface to view the stream
- Built with ESP-IDF 6.1

## Hardware

- ESP32-CAM module
- USB-to-UART adapter (for flashing)
- Camera (OV2640 - included with most ESP32-CAM boards)

## Setup Instructions

### 1. Configure WiFi Credentials

Edit [main/main.c](main/main.c) and update these lines:

```c
#define WIFI_SSID "YOUR_SSID"
#define WIFI_PASS "YOUR_PASSWORD"
```

### 2. Build the Project

```bash
cd /home/phys/Projects/esp32-cam
get_idf  # Load ESP-IDF environment
idf.py build
```

### 3. Flash the Firmware

Connect your ESP32-CAM via USB and run:

```bash
idf.py flash
```

### 4. Monitor Output

```bash
idf.py monitor
```

You should see output like:
```
I (123) esp32-cam-webserver: Connecting to WiFi...
I (456) esp32-cam-webserver: connected to ap SSID:YOUR_SSID
I (789) esp32-cam-webserver: got ip:192.168.X.X
I (1000) esp32-cam-webserver: Starting web server...
I (1100) esp32-cam-webserver: Web server started. Access at http://<ip>/
```

### 5. Access the Stream

Open your browser and navigate to: `http://<ESP32_IP_ADDRESS>/`

You should see a simple HTML page with an MJPEG stream from the camera.

## Project Structure

```
esp32-cam/
├── CMakeLists.txt              # Root CMake configuration
├── main/
│   ├── CMakeLists.txt          # Component build configuration
│   ├── idf_component.yml       # Component dependencies
│   ├── main.c                  # Main application code
│   └── include/                # Include directory (empty)
├── build/                      # Build artifacts (auto-generated)
└── sdkconfig                   # ESP-IDF configuration
```

## Key Components

- **WiFi**: Connects to AP in STA mode with automatic reconnection
- **HTTP Server**: Uses ESP-IDF HTTP server component on port 80
- **Camera**: Initializes OV2640 sensor with JPEG compression
- **MJPEG Stream**: Streams frames as multipart/x-mixed-replace

## Camera Pins (ESP32-CAM)

The camera is configured to use the following GPIO pins:
- XCLK: GPIO 0
- SIOD (SDA): GPIO 26
- SIOC (SCL): GPIO 27
- Y2-Y9: GPIO 5, 18, 19, 21, 36, 39, 34, 35
- VSYNC: GPIO 25
- HREF: GPIO 23
- PCLK: GPIO 22
- PWDN: GPIO 32

## Troubleshooting

### Device not appearing
- Check USB cable connection
- Ensure correct CH340 or CP2102 driver is installed
- Try: `idf.py flash -p /dev/ttyUSB0` (adjust port as needed)

### Camera initialization fails
- Check physical camera connection
- Verify power supply (camera needs ~100mA)
- Ensure camera ribbon cable is properly seated

### WiFi connection fails
- Double-check SSID and password
- Verify WiFi network is 2.4GHz (ESP32 doesn't support 5GHz)
- Check router logs for connection attempts

### Stream not loading
- Verify ESP32 has IP address (check serial output)
- Ping the device: `ping <ESP32_IP>`
- Check browser console for errors
- Try accessing root page first: `http://<ESP32_IP>/`

## Performance Notes

- Frame rate: ~15-20 FPS at QVGA resolution (320x240)
- Bandwidth: ~500-1000 kbps depending on JPEG quality
- Recommended for local network use

## Future Improvements

- Add REST API for camera settings (brightness, contrast, etc.)
- Support multiple frame sizes
- Add frame rate control
- Implement authentication
- Add OTA updates

## License

MIT License

