# 🎮 Gensyn Minecraft BlockAssist v0.1.0 on WSL ��️

**BlockAssist v0.1.0 has been released with important fixes, including a HuggingFace update that all users need to apply.**

⚡️ **Action Required:** Please wipe & reclone BlockAssist, then follow the official guide below.

---

## ✅ Prerequisites

- 🖥️ Windows 10/11 with WSL 2 installed  
- 🐧 Ubuntu (or your preferred Linux distro) installed in WSL  
- 🐍 Python 3.10+ installed  
- 🔧 Git installed  
- ��️ VcXsrv or another X server for Windows installed and running  
- 📦 Poetry for Python dependency management  
- 🟢 Node.js and Yarn for the login interface

---

## 📦 Installation Steps

## Step 1: Install VcXsrv (X Server for Windows) ��️

1. Download VcXsrv from [Download VcXsrv](https://sourceforge.net/projects/vcxsrv/)  
![image alt](https://github.com/gasoline2255/Gensyn-Blockassist-WSL-Guide/blob/d418a853957c0205d967b244e878cce093e85ee2/Screenshot%202025-08-09%20051036.png)

2. Run the installer and complete the installation 

---

## Step 2: Clone and Setup BlockAssist in WSL

Open your WSL terminal and run these commands **one by one**:

```bash
# Remove old installation (if exists)
rm -rf ~/blockassist
```

# Clone the repository and enter its folder
```
git clone https://github.com/gensyn-ai/blockassist.git
cd blockassist
```
# Install Core Dependencies
Install Java
```
./setup.sh
```
# Install and configure pyenv
```
curl -fsSL https://pyenv.run | bash
```
# Configure pyenv environment variables
```
export PYENV_ROOT="$HOME/.pyenv"
[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init - bash)"
eval "$(pyenv virtualenv-init -)"
```

# Reload your shell environment
```
source ~/.bashrc
```

# Update system and install dependencies (enter 'y' if prompted)
```
sudo apt update
sudo apt install -y make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev \
curl git libncursesw5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev
```

# Install Python 3.10 using pyenv and set it globally
```
pyenv install 3.10.0
pyenv global 3.10.0
```

# Install required Python packages
```
pip install psutil readchar
```

## Step 3: Install Node.js and Yarn


# Install Node.js via nvm (Node Version Manager)
```
curl -fsSL https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
```
# Install latest LTS Node.js and set as default
```
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"
nvm install --lts
nvm alias default 'lts/*'
nvm use default
```
# Enable Corepack and install Yarn
```
corepack enable
corepack prepare yarn@stable --activate
```
# Verify installations
```
node -v
npm -v
yarn -v
```

## Step 4: (Optional) Install and Configure cuDNN for Nvidia GPU Support
```
wget https://developer.download.nvidia.com/compute/cudnn/9.11.0/local_installers/cudnn-local-repo-ubuntu2204-9.11.0_1.0-1_amd64.deb
sudo dpkg -i cudnn-local-repo-ubuntu2204-9.11.0_1.0-1_amd64.deb
sudo cp /var/cudnn-local-repo-ubuntu2204-9.11.0/cudnn-local-4EC753EA-keyring.gpg /usr/share/keyrings/
echo "deb [signed-by=/usr/share/keyrings/cudnn-local-4EC753EA-keyring.gpg] file:///var/cudnn-local-repo-ubuntu2204-9.11.0 /" | sudo tee /etc/apt/sources.list.d/cudnn-local.list
sudo apt update
sudo apt install -y libcudnn9 libcudnn9-dev
```
# Add CUDA libraries to your path and reload
```
echo 'export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH' >> ~/.bashrc
source ~/.bashrc
```

## Step 5: Install Poetry (Python Dependency Manager)
```
curl -sSL https://install.python-poetry.org | python3 -
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
poetry --version
```

## Step 6: Install psutil, readchar, and rich

```bash
pip install psutil readchar rich
```

## Step 7: Configure VcXsrv to Accept Connections from WSL
**1.** Open CMD or PowerShell:
```bash
ipconfig
```
Find your IPv4 Address `(e.g., 192.168.1.10)`.

**2.** Launch VcXsrv with:

✅ Multi-window

✅ Start no client

✅ Disable access control

❌ Uncheck "Native OpenGL"

**3.**
In WSL:
**Install X11 tools**
```
sudo apt-get update -y
sudo apt-get install -y x11-apps x11-xserver-utils
```
**Set DISPLAY to your Windows IP (Example: export DISPLAY=192.168.1.10:0)**

```
export DISPLAY=<WINDOWS_IP>:0
```
**Test basic X11**
```
xeyes
```
**Check screen modes**
```
xrandr -q
```

4. Fix OpenGL issues (software rendering):
**Force Mesa software GL**
```
export LIBGL_ALWAYS_SOFTWARE=1
export MESA_LOADER_DRIVER_OVERRIDE=llvmpipe
export LIBGL_ALWAYS_INDIRECT=1
export MESA_GL_VERSION_OVERRIDE=2.1
```
**Allow LWJGL 2 to use software OpenGL**
```
export _JAVA_OPTIONS='-Xms512m -Xmx2g -Dorg.lwjgl.opengl.Display.allowSoftwareOpenGL=true'
```

## Step 8: Login to Gensyn Modal and Run BlockAssist

**1** First, go to the modal-login directory and start the login server:
```
cd modal-login
yarn install
yarn dev
```

**2** Open your browser at `http://localhost:3000` and log in to Gensyn. 

**After login enter ```Ctrl+C``` on your terminal and run:**
```bash
cd
cd blockassist
```

## Step 9: Activate Environment & Install BlockAssist Locally (Malmo Fix) 🛠️

**1)** Activate the same venv
```
python3 -m venv blockassist-venv
source blockassist-venv/bin/activate
```
**2)** Make sure build tools are fine
```
python -m pip install --upgrade pip setuptools wheel
```
**3)** Install the repo in editable mode so Python can import its packages (including `malmo`)
```
pip install -e .
```

**Check if malmo is available**
```
python - <<'PY'
import pkgutil
print('malmo' in [m.name for m in pkgutil.iter_modules()])
PY
```

**4** Now run BlockAssist:
```bash
python3 run.py
```

**5** Now it will ask for Hugging Face token: `Paste your token`

![image alt](https://github.com/gasoline2255/Gensyn-Blockassist-WSL-Guide/blob/main/Screenshot%202025-09-09%20232659.png)

**6** And soon your Minecraft game will open in VcXsrv

## 🚀 How To Start BlockAssist the Next Day

Follow these steps whenever you want to restart BlockAssist after shutdown:

```bash
cd blockassist

# Configure pyenv environment variables
export PYENV_ROOT="$HOME/.pyenv"
[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init - bash)"
eval "$(pyenv virtualenv-init -)"
```
##Launch VcXsrv on Windows with:
```bash
✅ Multi-window
✅ Start no client
✅ Disable access control
```
Set DISPLAY to your Windows IP
```bash
export DISPLAY=<WINDOWS_IP>:0 
```
 Test X11 forwarding (a small "eyes" window should appear, then close it)
```bash
xeyes
```
Now run BlockAssist:
```bash
source blockassist-venv/bin/activate
python3 run.py
```
