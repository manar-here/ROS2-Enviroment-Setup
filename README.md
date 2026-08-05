# ROS2-Enviroment-Setup
# ROS2 Humble Environment Setup & Architecture Guide

A comprehensive technical guide for installing, configuring, and verifying the **ROS2 Humble Hawksbill** environment on Ubuntu Linux — tailored for aerospace and robotics development workflows.

![ROS2](https://img.shields.io/badge/ROS2-Humble-blue)
![Ubuntu](https://img.shields.io/badge/Ubuntu-22.04%20LTS-orange)
![License](https://img.shields.io/badge/license-MIT-green)

---

## Table of Contents

- [System Requirements & Prerequisites](#system-requirements--prerequisites)
- [Installation Workflow](#installation-workflow)
- [Environment Configuration & Persistence](#environment-configuration--persistence)
- [Verification & Health Check](#verification--health-check)
- [Recommended Workspace Initialization](#recommended-workspace-initialization)
- [Troubleshooting](#troubleshooting)
- [Official References & Resources](#official-references--resources)
- [License](#license)

---

## System Requirements & Prerequisites

| Requirement | Details |
|---|---|
| **Operating System** | Ubuntu 22.04 LTS (Jammy Jellyfish) |
| **Framework Version** | ROS2 Humble Hawksbill |
| **Target Domain** | Robotics, Avionics, and Autonomous Systems |
| **Disk Space** | ~5 GB free (Desktop Full install) |
| **Privileges** | `sudo` access required |

---

## Installation Workflow

### 1. Update System Packages and Install Curl

Update your package lists and install the `curl` utility:

```bash
sudo apt update && sudo apt install curl -y
```

### 2. Register the ROS2 GPG Key

Create the secure keyrings directory and download the official ROS2 GPG key to authenticate packages:

```bash
sudo mkdir -p /usr/share/keyrings
sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key \
  -o /usr/share/keyrings/ros-archive-keyring.gpg
```

### 3. Add the ROS2 Repository to System Sources

Append the ROS2 repository to your package source list, using architecture-specific filters:

```bash
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] \
http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | \
  sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null
```

### 4. Install ROS2 Humble Desktop Full Package

Refresh the package index with the newly added repository, then install the complete desktop environment (including visualization tools like RViz2 and simulation packages):

```bash
sudo apt update
sudo apt install ros-humble-desktop -y
```

> **Screenshot:** *Package installation & download progress.*
> `docs/screenshots/01-installation.png`

---

## Environment Configuration & Persistence

To automatically load ROS2 environment variables in every new terminal session (without manual sourcing), append the setup script to your shell profile:

```bash
echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

> **Screenshot:** *Environment sourcing confirmation.*
> `docs/screenshots/02-environment-sourcing.png`

---

## Verification & Health Check

Confirm the installation succeeded and that ROS2 commands are recognized system-wide:

```bash
ros2 --help
```

**Expected outcome:** a full help listing of core ROS2 sub-commands (`action`, `bag`, `node`, `param`, `topic`, etc.) should render immediately.

> **Screenshot:** *CLI verification output.*
> `docs/screenshots/03-cli-verification.png`

---

## Recommended Workspace Initialization

Initialize a standard `colcon` workspace to begin developing custom nodes and packages:

```bash
mkdir -p ~/ros2_ws/src
cd ~/ros2_ws/
colcon build
```

Once built, don't forget to source the local workspace overlay in addition to the global ROS2 install:

```bash
echo "source ~/ros2_ws/install/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

---

## Troubleshooting

| Issue | Likely Cause | Fix |
|---|---|---|
| `ros2: command not found` | Environment not sourced | Re-run `source /opt/ros/humble/setup.bash` or open a new terminal |
| GPG key errors during `apt update` | Keyring file missing/corrupted | Repeat Step 2 to re-download the key |
| `colcon: command not found` | `colcon` not installed | `sudo apt install python3-colcon-common-extensions -y` |
| Repository 404 errors | Wrong Ubuntu codename detected | Verify with `lsb_release -cs`; it must return `jammy` |

---

## Official References & Resources

- [Official ROS2 Humble Documentation](https://docs.ros.org/en/humble/index.html)
- [ROS2 Open-Source Organization on GitHub](https://github.com/ros2)

---

## License

This guide is released under the [MIT License](LICENSE).
