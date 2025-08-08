## Gensyn Minecraft BlockAssist on WSL
This guide helps you set up and run Gensyn Minecraft BlockAssist within a WSL environment.

## Prerequisites
- Windows 10/11 with WSL 2 installed

- Ubuntu (or your preferred Linux distro) installed in WSL

- Python 3.10+ installed

- Git installed

- VcXsrv or other X server for Windows installed and running

- poetry for Python dependency management

## Setup Steps
1. Download and Install VcXsrv
https://sourceforge.net/projects/vcxsrv/
Download the installer from VcXsrv on SourceForge

2. Run the installer and complete the installation

## Step: Find Your Windows Host IP Address
To enable graphical apps in WSL to display properly (for example, when using an X server like VcXsrv), you need your Windows host IP address.
1. Open Command Prompt `(CMD)` on your Windows machine:

`Press Win + R, type cmd, and press Enter.`

2. Run the following command: `ipconfig`
