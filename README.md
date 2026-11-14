# TITAN Task Manager — Arch Linux Run Kit

**Purpose:** Run the developing C++20 + Qt6/QML TITAN Task Manager on ordinary Arch Linux without building ArchTitan OS or running THM. This kit contains only a build/launch helper and documentation; **it does not include the Task Manager source**.

## 1. Install Arch dependencies

```bash
sudo pacman -Syu
sudo pacman -S --needed base-devel cmake ninja qt6-base qt6-declarative unzip
```

Run commands as your **normal desktop user**, not root.

## 2. Extract the Task Manager source ZIP

Use your existing latest TITAN Task Manager ZIP (for example the v0.7 source from the previous message). Extract it into a folder such as `~/dev/TITAN/`. The project lives under:

`~/dev/TITAN/subsystems/titan-task-manager/CMakeLists.txt`

This separate kit does *not* need the ArchTitan OS checkout. Do not copy compiled `libTitanUI.so` from the ArchTitan ISO image into your host installation; it may be incompatible with your Qt build.

## 3. Build and run

Download and extract this kit, then run its script with the project path:

```bash
./run-on-arch-linux.sh ~/dev/TITAN/subsystems/titan-task-manager run
```

Or run the commands manually from inside `subsystems/titan-task-manager`:

```bash
cmake -S . -B build -G Ninja -DCMAKE_BUILD_TYPE=Debug
cmake --build build
ctest --test-dir build --output-on-failure
./build/titan-task-manager
```

## Optional: tests, demo data, and desktop launcher

```bash
./run-on-arch-linux.sh ~/dev/TITAN/subsystems/titan-task-manager test
./run-on-arch-linux.sh ~/dev/TITAN/subsystems/titan-task-manager demo
./run-on-arch-linux.sh ~/dev/TITAN/subsystems/titan-task-manager install-desktop
```

The `demo` action supplies fake THM telemetry. Stop any already running instance first; it uses single-instance IPC, so a second launch may simply activate the existing process.

## Regular Arch Linux versus ArchTitan OS

- **Available without THM:** Overview CPU/RAM graphs, `/proc` Processes table, command and resource inspection.
- **Hyprland-dependent:** Workspace focus tracking requires a Hyprland session.
- **ArchTitan-only:** THM workload management, THM decisions, and ArchTitan cgroup slice metrics require their respective services.
- **Not enabled:** Actual process termination. The safety preview does not send signals.

Your project's temporary dark QML theme is intentionally self-contained and does not require the official `ArchTitan.TitanUI` binary.

## Troubleshooting

- **`Could not find Qt6Config.cmake`**: install `qt6-base` and `qt6-declarative`.
- **`QtQuick.Controls` / QML import errors**: ensure `qt6-declarative` is installed. Start from a desktop session; use `QML_IMPORT_TRACE=1` for diagnostics.
- **`XDG_RUNTIME_DIR` missing**: run from a graphical login session, not a detached SSH shell or `sudo`.
- **THM unavailable**: expected on ordinary Arch Linux. Use **Overview** and **Processes** to confirm local monitoring.
- **Single-instance app doesn't reopen**: stop its existing process, then launch again.

**Validation:** Script syntax checked. The app itself wasn't compiled or launched here because Qt6 development packages aren't installed. Report any CMake/CTest/QML diagnostics from your Arch machine.
