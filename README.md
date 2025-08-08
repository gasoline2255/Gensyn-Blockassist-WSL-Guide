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

3. Look for the section named Ethernet adapter or Wi-Fi (depending on your connection).

4. Find the IPv4 Address. It will look something like: ` IPv4 Address. . . . . . . . . . . : 192.168.1.10`

## Run These Commands One by One in Your WSL Terminal

Run these commands **one by one** inside your WSL terminal:

1. ### Clone the repo and enter its directory:
   ```bash
   git clone https://github.com/gensyn-ai/blockassist.git
   cd blockassist
2. ### Run the setup script: 
```bash
./setup.sh
```
3. ### Install Pyenv:
```bash
curl -fsSL https://pyenv.run | bash
```
4. ### Configure Pyenv environment variables:
```bash
export PYENV_ROOT="$HOME/.pyenv"
[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init - bash)"
eval "$(pyenv virtualenv-init -)"
```
5. ### Reload your shell:
```bash
source ~/.bashrc
```
6. ### Update system and install dependencies (enter `y` if prompted):
```bash
sudo apt update
sudo apt install make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev curl git libncursesw5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev
pyenv install 3.10
```
7. ### Install Python packages:
```bash 
pip install psutil readchar
```
8. ### Update cuDNN (for Nvidia GPU support):
```bash
wget https://developer.download.nvidia.com/compute/cudnn/9.11.0/local_installers/cudnn-local-repo-ubuntu2204-9.11.0_1.0-1_amd64.deb
sudo dpkg -i cudnn-local-repo-ubuntu2204-9.11.0_1.0-1_amd64.deb
sudo cp /var/cudnn-local-repo-ubuntu2204-9.11.0/cudnn-local-4EC753EA-keyring.gpg /usr/share/keyrings/
echo "deb [signed-by=/usr/share/keyrings/cudnn-local-4EC753EA-keyring.gpg] file:///var/cudnn-local-repo-ubuntu2204-9.11.0 /" | sudo tee /etc/apt/sources.list.d/cudnn-local.list
sudo apt update
sudo apt install -y libcudnn9 libcudnn9-dev
```
9. ### Add CUDA libraries to your path and reload:
```bash
echo 'export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH' >> ~/.bashrc
source ~/.bashrc
```
10. ### Install Poetry (Python Dependency Manager)

Run the following command to install Poetry globally:

```bash
curl -sSL https://install.python-poetry.org | python3 -
```
After installation, add Poetry to your PATH by adding this to your shell config (`~/.bashrc` or `~/.zshrc`):
```bash
export PATH="$HOME/.local/bin:$PATH"
```
Then reload your shell:
```bash
source ~/.bashrc
```
Verify Poetry is installed correctly:
```bash
poetry --version
```

### Configure VcXsrv to Accept Connections from WSL

1. Find your Windows host IP: Open Windows Command Prompt (`Win + R`, then type `cmd`) and run:

```bash
ipconfig
```
Look for the **IPv4 Address** under your active network adapter (e.g., Wi-Fi or Ethernet). Example: `192.168.1.10`

2. Launch VcXsrv on Windows:

Start VcXsrv with these settings:

- Select Multiple windows

- Select Start no client

- Check Disable access control (to allow connections)

- Finish the setup and launch the server

3. **Set the DISPLAY environment variable inside WSL:**

Run this command in your WSL terminal, replacing `<WINDOWS_IP>` with your actual IPv4 address from step 1:
```bash 
export DISPLAY=<WINDOWS_IP>:0
```
Example:
```bash 
export DISPLAY=192.168.1.10:0
```
4. (Optional) To make this persistent, add it to your `~/.bashrc` or `~/.profile`:
```bash 
echo "export DISPLAY=$(grep -m 1 nameserver /etc/resolv.conf | awk '{print $2}'):0" >> ~/.bashrc
source ~/.bashrc
```
Now we need to check X server is running and the firewall allows connections.
```bash 
xeyes
```



















