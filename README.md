# 🎮 Gensyn Minecraft BlockAssist on WSL 🛠️

This guide helps you set up and run **Gensyn Minecraft BlockAssist** within a Windows Subsystem for Linux (WSL) environment.

---

## ✅ Prerequisites

- 🖥️ Windows 10/11 with WSL 2 installed  
- 🐧 Ubuntu (or your preferred Linux distro) installed in WSL  
- 🐍 Python 3.10+ installed  
- 🔧 Git installed  
- 🖼️ VcXsrv or another X server for Windows installed and running  
- 📦 Poetry for Python dependency management  

---

## Step 1: Install VcXsrv (X Server for Windows) 🖥️

1. Download VcXsrv from [Download VcXsrv](https://sourceforge.net/projects/vcxsrv/)  
2. Run the installer and complete the installation 

---

## Step 2: Clone and Setup BlockAssist in WSL

Open your WSL terminal and run these commands **one by one**:

```bash
# Clone the repository and enter its folder
git clone https://github.com/gensyn-ai/blockassist.git
cd blockassist

# Run the setup script
./setup.sh

# Install pyenv
curl -fsSL https://pyenv.run | bash

# Configure pyenv environment variables
export PYENV_ROOT="$HOME/.pyenv"
[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init - bash)"
eval "$(pyenv virtualenv-init -)"

# Reload your shell environment
source ~/.bashrc

# Update system and install dependencies (enter 'y' if prompted)
sudo apt update
sudo apt install -y make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev curl git libncursesw5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev

# Install Python 3.10 using pyenv and set it globally
pyenv install 3.10.0
pyenv global 3.10.0

# Install required Python packages
pip install psutil readchar
```
## Step 3: (Optional) Install and Configure cuDNN for Nvidia GPU Support
```bash
wget https://developer.download.nvidia.com/compute/cudnn/9.11.0/local_installers/cudnn-local-repo-ubuntu2204-9.11.0_1.0-1_amd64.deb
sudo dpkg -i cudnn-local-repo-ubuntu2204-9.11.0_1.0-1_amd64.deb
sudo cp /var/cudnn-local-repo-ubuntu2204-9.11.0/cudnn-local-4EC753EA-keyring.gpg /usr/share/keyrings/
echo "deb [signed-by=/usr/share/keyrings/cudnn-local-4EC753EA-keyring.gpg] file:///var/cudnn-local-repo-ubuntu2204-9.11.0 /" | sudo tee /etc/apt/sources.list.d/cudnn-local.list
sudo apt update
sudo apt install -y libcudnn9 libcudnn9-dev

# Add CUDA libraries to your path and reload
echo 'export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH' >> ~/.bashrc
source ~/.bashrc
```
## Step 4: Install Poetry (Python Dependency Manager)
```bash
curl -sSL https://install.python-poetry.org | python3 -
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
poetry --version
```
## Step 5: Configure VcXsrv to Accept Connections from WSL
**1.** Open Command Prompt (CMD) and run:
```bash
ipconfig
```
**2** Find your **Ethernet adapter vEthernet (WSL (Hyper-V firewall))**: eg `IPv4 Address. . . . . . . . . . . : 192.168.1.10`

**3** Launch VcXsrv on Windows with these settings:
- Select Multiple windows

- Select Start no client

- Check Disable access control

- Finish and launch the server

**4** In your WSL terminal, set the DISPLAY environment variable (replace `<IPV4 Address>`)
```bash
export DISPLAY=<WINDOWS_IP>:0
```
**Example:** `export DISPLAY=192.168.1.10:0`

**5** Make DISPLAY persistent, run:
```bash
echo "export DISPLAY=$(grep -m 1 nameserver /etc/resolv.conf | awk '{print $2}'):0" >> ~/.bashrc
source ~/.bashrc
```
**6** Test X server connection:
```bash
xeyes
```
**Now a xeyes window pop up. and you can close that window `cntrl +c`**


## Step 6: Login to Gensyn Modal and Run BlockAssist
**1** Bypass Gensyn login for local development:
```bash
cd modal-login
yarn dev
```
**2** Open your browser a `http://localhost:3000` and log in to Gensyn.
**3** Run BlockAssist:
```bash
python run.py
```
**4** Now it will ask Hugging Face token: ` Paste your token`

![image alt](https://github.com/gasoline2255/Gensyn-Blockassist-WSL-Guide/blob/60414bc20fe061df0f35536dd29fc85d0459e285/Screenshot%202025-08-09%20044704.png)

**5** And soon your minecraft game wil open in VcXsrv
