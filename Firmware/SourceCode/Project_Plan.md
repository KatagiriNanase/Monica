# Monica Firmware Source Code Analysis and Development Plan

## 1. Project Overview
This project appears to be the firmware for a device named "Monica". It uses the ESP-IDF framework (specifically v5.0.2) and relies on several components, most notably the `mooncake` UI framework and `lvgl` for graphics.

## 2. Architecture & Main Modules

### 2.1 Hardware Abstraction Layer (HAL)
Located in `main/hal/`, this module abstracts the underlying hardware components.
- **Submodules:**
  - `arduino`: Likely provides Arduino core compatibility or wrappers.
  - `button`: Button input handling.
  - `buzzer`: Buzzer/audio output control.
  - `disp`: Display driver/initialization.
  - `lvgl`: LVGL porting/integration with the display and touch.
  - `power`: Power management (battery, charging, etc.).
  - `rtc`: Real-Time Clock for timekeeping.
  - `sd_card`: SD Card interfacing.
  - `tp`: Touch panel driver.
  - `usb_msc`: USB Mass Storage Class implementation.

### 2.2 Hardware Manager
Located in `main/hardware_manager/`, this acts as a central coordinator for the HAL, initializing and updating hardware states.

### 2.3 UI Framework (Mooncake & LVGL)
- The project heavily uses `components/mooncake/`, which seems to be an application management framework built on top of LVGL.
- LVGL (`components/lvgl/`) is the core graphics library.
- The main application logic (`main/monica.cpp`) involves setting up `mooncake_ui`, installing built-in apps, and registering various applications (currently instantiated as `AppTest` with different icons/names).

### 2.4 User Apps
Located in `main/user_apps/`, this directory is intended to hold the individual applications or screens that the user can interact with through the Mooncake UI.

### 2.5 Dependencies
- **ESP-IDF:** v5.0.2
- **Mooncake:** Application framework.
- **LVGL:** Graphics library.
- **LovyanGFX:** Likely used as the display driver backend for LVGL.
- **ArduinoJson:** For JSON parsing/generation.
- **BMI270_BMM150:** IMU (Accelerometer, Gyroscope, Magnetometer) drivers.

## 3. Development Plan

### Phase 1: Environment Setup & Build Verification
1.  **Install ESP-IDF:** Ensure ESP-IDF v5.0.2 is installed and configured in your environment.
2.  **Clone Dependencies:** Run `git submodule init` and `git submodule update`.
3.  **Checkout Mooncake:** Navigate to `components/mooncake/` and `git checkout main`.
4.  **Build and Flash:** Run `idf.py build`, `idf.py flash`, and `idf.py monitor` to ensure the base firmware compiles and runs on the target hardware.

### Phase 2: Understanding the UI Framework (Mooncake)
1.  **Study `AppTest`:** Analyze the `AppTest` class in `main/monica.cpp` to understand the lifecycle of a Mooncake app (`onCreate`, `onResume`, `onRunning`, `onPause`, `onDestroy`).
2.  **Explore Built-in Apps:** Investigate `mooncake_ui.installBuiltinApps()` to see what functionalities are provided out-of-the-box.
3.  **Create a Simple App:** Create a new folder in `main/user_apps/`, define a new class inheriting from `MOONCAKE::APP_BASE`, and register it in `app_main`.

### Phase 3: Hardware Integration
1.  **Explore HAL:** Review the files in `main/hal/` to understand how the hardware is accessed.
2.  **Interact with Hardware from an App:** Modify your test app to read a button state or display information from the RTC/IMU using the `hardware_manager`.

### Phase 4: Feature Development
1.  **Define Requirements:** Determine the specific applications or features you want to build for the Monica device.
2.  **Implementation:** Develop the applications within the `main/user_apps/` structure.
3.  **Testing:** Continuously test on the hardware to ensure stability and performance.