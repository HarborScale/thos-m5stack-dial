# HFOS v2.1 – M5Stack Dial Firmware

<!-- OSS Badges -->

![License](https://img.shields.io/github/license/TelemetryHarbor/HFOS-m5stack-dial.svg)
![Last Commit](https://img.shields.io/github/last-commit/TelemetryHarbor/HFOS-m5stack-dial.svg)
![Issues](https://img.shields.io/github/issues/TelemetryHarbor/HFOS-m5stack-dial.svg)
![Pull Requests](https://img.shields.io/github/issues-pr/TelemetryHarbor/HFOS-m5stack-dial.svg)
![Repo Size](https://img.shields.io/github/repo-size/TelemetryHarbor/HFOS-m5stack-dial.svg)
![Contributors](https://img.shields.io/github/contributors/TelemetryHarbor/HFOS-m5stack-dial.svg)
![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)
![Stars](https://img.shields.io/github/stars/TelemetryHarbor/HFOS-m5stack-dial.svg?style=social)
![Forks](https://img.shields.io/github/forks/TelemetryHarbor/HFOS-m5stack-dial.svg?style=social)

HFOS is a fully-featured open-source firmware for the **M5Stack Dial**.
It brings **watch, alarm, timer, and settings apps** in a lightweight, smooth, and responsive UI with **OLED graphics**, rotary encoder input, and tactile feedback.

**NEW:** Optional **productivity tracking dashboard** via HarborScale integration – track your focus sessions in real-time with beautiful Grafana visualizations! 📊



## 🔥 Features

* ⌚ **Smart Watch** – Highly optimized, low-power clock with 12/24h toggle
* ⏱️ **Timer** – Set, start, pause, and ring timers with gauge progress
* ⏰ **Alarm** – Polling-based alarm with snooze and dynamic ringing alerts
* ⚙️ **Settings** – Wi-Fi NTP sync, 12/24h mode toggle, brightness control
* 📊 **Productivity Dashboard (Optional)** – Track timer sessions with HarborScale + Grafana
* 🎨 **Industrial Palette UI** – Modern, minimal, and vibrant design
* 📈 **Smart Redraw & Efficient Background Tasks** – Minimal CPU usage
* 🛠️ **Extensible App System** – Easy to add new apps via the `App` interface
* 🎵 **Audio Alerts** – Tone support via M5Dial speaker
* 🌙 **AOD Mode** – Screensaver with low-brightness display
* 💡 **Rotary & Touch Input Support** – Smooth, debounced interaction
* ⌨️ **USB HID Control** – Send Ctrl+1 (Start) and Ctrl+2 (Stop) to your computer



## 🚀 Quick Start

1. Clone the repository:
```bash
git clone https://github.com/harborscale/HFOS-m5stack-dial.git
cd HFOS-m5stack-dial
```

2. Open in **Arduino IDE** or **PlatformIO**.
3. Install required libraries:

   * `M5Dial`
   * `M5GFX`
   * `HTTPClient` (for optional HarborScale integration)
4. Configure your Wi-Fi and timezone in `main.cpp`:
```cpp
const char* WIFI_SSID = "YOUR_SSID";     
const char* WIFI_PASS = "YOUR_PASSWORD"; 
#define TIME_ZONE_OFFSET_HRS +3
```

5. **(Optional)** Enable productivity tracking:
```cpp
const char* HARBOR_API_KEY = "your_harborscale_api_key";
const char* HARBOR_API_ENDPOINT = "https://harborscale.com/api/v2/ingest/your_harbor_id";
const char* HARBOR_SHIP_ID = "Productivity_Dial";
```

6. Build and upload to your **M5Stack Dial**.



## 📊 Productivity Dashboard (Optional)

HFOS can automatically track your timer sessions and visualize them in a beautiful dashboard using [HarborScale](https://harborscale.com) and Grafana.

### How It Works

* **When timer starts:** Sends status `1` to HarborScale
* **Every 60 seconds:** Sends heartbeat status `1` (shows continuous activity)
* **When timer stops/finishes:** Sends status `0` to HarborScale

### Setup Instructions

1. **Create a free HarborScale account** at [harborscale.com](https://harborscale.com)
2. **Create a General Harbor** and get your Harbor ID
3. **Generate an API Key** from your dashboard
4. **Configure HFOS** with your credentials (see Quick Start step 5)
5. **View a Grafana dashboard** to visualize:
   * Total productive time per day
   * Session duration trends
   * Focus time heatmaps
   * Weekly/monthly productivity stats

**Want to skip the setup?** Just leave the `HARBOR_*` variables empty and HFOS works perfectly without tracking! 🎯



## 📋 Supported Hardware

* **M5Stack Dial**

  * M5Core ESP32
  * Built-in OLED 240×240
  * Rotary encoder + button
  * Speaker for alarms
  * USB-C (HID keyboard support)



## ⚡ UI & Interaction

* **Menu Navigation**

  * Rotate encoder → Switch apps
  * Tap screen → Select / confirm
  * Rotart Press → Back / exit app

* **Watch App**

  * Smart redraw to save power
  * Shows date, time, and AM/PM if 12h mode

* **Timer App**

  * Rotary = adjust time (coarse/fine)
  * Tap = start/pause/stop
  * Ringing = dynamic gauge + tone
  * Sends USB keyboard commands (Ctrl+1/Ctrl+2)
  * (Optional) Tracks sessions to HarborScale

* **Alarm App**

  * Rotary = toggle ON/OFF or adjust hour/min
  * Tap = snooze when ringing
  * Background polling every 500ms for accurate alarm trigger

* **Settings App**

  * Tap top half = NTP sync over Wi-Fi
  * Tap bottom half = toggle 12/24h format
  * Feedback tones for success/failure



## 🔧 Configuration Options

| Parameter                | Description                                   | Default                                     |
| ------------------------ | --------------------------------------------- | ------------------------------------------- |
| `TIME_ZONE_OFFSET_HRS`   | Offset from UTC                               | +3                                          |
| `BRIGHT_ACTIVE`          | Active display brightness (0–255)             | 200                                         |
| `BRIGHT_AOD`             | Screensaver/AOD brightness (0–255)            | 10                                          |
| `SCREENSAVER_TIMEOUT`    | Inactivity before AOD (ms)                    | 20000                                       |
| `C_ACCENT`               | Main accent color                             | 0xFB20                                      |
| `HARBOR_API_KEY`         | HarborScale API key (optional)                | ""                                          |
| `HARBOR_API_ENDPOINT`    | HarborScale ingest endpoint (optional)        | "https://harborscale.com/api/v2/ingest/..." |
| `HARBOR_SHIP_ID`         | Device identifier for tracking (optional)     | "Productivity_Dial"                         |



## 🛠️ Extending HFOS

HFOS uses an `App` interface for modularity.
Add your own app:
```cpp
class MyApp : public App {
    void setup() override {}
    void loop() override {}
    void draw() override {}
    void onRotary(int delta) override {}
    void onTouch(int x, int y) override {}
    String getName() override { return "MY APP"; }
};
```

Then register in `setup()`:
```cpp
apps.push_back(new MyApp());
```



## ⚠️ Notes

* Wi-Fi NTP sync may take a few seconds; firmware handles retry automatically.
* Timer & Alarm override menu for priority alerts.
* Rotary encoder sensitivity can be adjusted by changing `abs(rawDelta) >= 4` threshold.
* **Productivity tracking is completely optional** – device works perfectly without HarborScale credentials
* HarborScale integration uses minimal WiFi connections (only when timer state changes)



## 📜 License

This project is licensed under **Apache License 2.0**.

* ✅ Free for personal and commercial use
* ✅ Modify and redistribute allowed
* ⚠️ Must include attribution



## 🤝 Contributing

We welcome issues, pull requests, and feature ideas!

* Open a GitHub issue for bugs or enhancements
* Fork the repo and submit PRs
* Join discussions and improve HFOS together



## 🙏 Acknowledgments

* Inspired by the [TimeChi Kickstarter project](https://www.indiegogo.com/en/projects/seangreenhalgh-15576326/timechi-your-smart-productivity-tool) & [salimbenbouz](https://www.instructables.com/member/salimbenbouz/)
* Powered by [HarborScale](https://harborscale.com) for telemetry tracking
* Built for the amazing M5Stack Dial community
