# TouchDesigner Watchdog

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![TD Version](https://img.shields.io/badge/TouchDesigner-2023.12120-blue.svg)](https://derivative.ca/)

English | [日本語](README_ja.md)

A watchdog system for TouchDesigner projects that monitors target applications and automatically restarts them if heartbeat signals are lost.

<img width="563" alt="image" src="https://github.com/user-attachments/assets/47a6290d-6911-41e4-bb40-535a8fe0858f" />

## Table of Contents
- [Features](#features)
- [Requirements](#requirements)
- [File Structure](#file-structure)
- [Setup](#setup)
- [Usage](#usage)
- [How It Works](#how-it-works)
- [Sample Program](#sample-program)
- [Troubleshooting](#troubleshooting)
- [Important Notes](#important-notes)
- [Version History](#version-history)
- [License](#license)

## Features
- 💫 Automatic monitoring of TouchDesigner projects
- 🔄 Automatic restart on crash
- 📡 Lightweight UDP-based heartbeat communication
- 🔌 Easy integration (drag & drop)

## Requirements

- TouchDesigner 2023.12120 (verified working version, may work with other versions as well)
- macOS (Windows is not supported)

## File Structure

- `watchdog.toe` - Main watchdog system that monitors heartbeat signals from target applications and handles process termination/restart when signals are lost
- `watchdog-heartbeat-sender.tox` - Heartbeat sender component to be integrated into target applications
- `watchdog-target.toe` - Sample application with the heartbeat sender component integrated

## Setup

### 1. Target Application Setup

1. Open your target TouchDesigner project
2. Drag and drop `watchdog-heartbeat-sender.tox` directly into the TouchDesigner project window
   - The component will be automatically added to your project
3. Select the added component and configure the following settings:
   - IP Address: IP address of the PC running the watchdog system
   - Port Number: UDP port number to use
4. Save your project

### 2. Watchdog Setup

1. Open `watchdog.toe`
2. Configure the Target Application section:
   - Absolute Path: Set the path to your target application
     - Default value is `/path/to/watchdog-target.toe`, change this to the actual path of your target file
   - Port Number: Set the same port number as configured in the heartbeat sender

## Usage

1. Launch your target TouchDesigner project
2. Launch `watchdog.toe`
3. The watchdog system will automatically start monitoring heartbeat signals
4. If the target project stops responding:
   - The process will be forcefully terminated
   - It will be automatically restarted

## How It Works

1. The `watchdog-heartbeat-sender.tox` component integrated into the target project sends UDP packets periodically
2. `watchdog.toe` monitors these heartbeat signals
3. If no heartbeat signals are received for 30 seconds, it determines there's an issue
4. When an issue is detected:
   - The target process is forcefully terminated
   - The restart process is automatically executed

## Sample Program

`watchdog-target.toe` is a sample application with the heartbeat sender component already integrated. You can use this as a reference for implementing the system in your own projects.

## Troubleshooting

### Heartbeat Signal Not Detected
- Check firewall settings
- Verify IP address and port number configurations
- Check network connectivity

### Process Restart Fails
- Verify absolute path settings
- Check file permissions

## Important Notes

- Ensure UDP communication is allowed in your network settings (firewall, etc.)
- Process paths must be specified as absolute paths
- Thoroughly test the system before using it in a production environment
- This system currently only supports macOS. Windows support is not available at this time
- Use this system at your own risk. The author(s) are not responsible for any damages or losses caused by the use of this system

## Version History

- v1.0.0
  - Initial release
  - Basic watchdog functionality
  - macOS support

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
