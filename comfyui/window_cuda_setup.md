# **Setting Up ComfyUI on Windows with CUDA (NVIDIA GPU)**

This guide walks through a full manual setup of **ComfyUI** on a Windows machine with an NVIDIA GPU and CUDA support.

---

## **Table of Contents**

1. Pre-requisites
2. ComfyUI Setup
3. Optional Components
4. Creating a Launch BAT File

---

## **1. Pre-requisites**

### **1.1 Python**

Check whether Python is installed:

```
python --version
```

or

```
python3 --version
```

If Python is not recognized:

* Install **Python 3.12** from:
  [https://www.python.org/downloads/release/python-31212/](https://www.python.org/downloads/release/python-31212/)
* Ensure **Add Python to PATH** is enabled during installation.

---

### **1.2 Git for Windows**

Verify Git installation:

```
git --version
```

If not installed, download from:
[https://git-scm.com/install/windows](https://git-scm.com/install/windows)

During installation, ensure Git is added to PATH.

---

### **1.3 Virtual Environment Setup**

Create a dedicated directory for ComfyUI.
Example: `C:\comfy`

Create the virtual environment:

```
python -m venv comfy
```

Activate it:

```
C:\comfy\comfy\Scripts\activate.bat
```

---

### **1.4 CUDA Toolkit 13.0**

Download and install the CUDA Toolkit:

[https://developer.nvidia.com/cuda-toolkit](https://developer.nvidia.com/cuda-toolkit)

Complete installation using the NVIDIA wizard.

---

## **2. ComfyUI Setup**

> **Important:** Ensure the virtual environment is activated before running any Python or pip commands.

---

### **2.1 Download ComfyUI**

Clone the repository:

```
git clone https://github.com/comfyanonymous/ComfyUI.git C:\GenAI\comfy
```

Repository reference:
[https://github.com/comfyanonymous/ComfyUI](https://github.com/comfyanonymous/ComfyUI)

---

### **2.2 Install Required Python Packages**

Navigate to the cloned folder:

```
cd C:\comfy\ComfyUI
```

Install required dependencies:

```
pip install -r requirements.txt
```

---

### **2.3 Install PyTorch (CUDA 13.0 Build)**

The default torch installation does not include CUDA support.
Remove it first:

```
pip uninstall torch -y
```

Install the CUDA-compatible version:

```
pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu130
```

ComfyUI is now ready to run with CUDA acceleration.

---

### **2.4 Test ComfyUI Launch**

Run:

```
python main.py
```

If ComfyUI starts successfully, the setup is correct.

---

## **3. Optional Components**

### **3.1 Install Triton**

```
pip install -U triton-windows
```

---

### **3.2 Install SAGE**

Download SAGE from the release page:
[https://github.com/woct0rdho/SageAttention/releases](https://github.com/woct0rdho/SageAttention/releases)

If using **CUDA 13.0** and **PyTorch 2.9.0**, download:

```
sageattention-2.2.0+cu130torch2.9.0.post3-cp39-abi3-win_amd64.whl
```

Copy the `.whl` file into the ComfyUI directory, then install it:

```
cd C:\comfy\ComfyUI
pip install "sageattention-2.2.0+cu130torch2.9.0.post3-cp39-abi3-win_amd64.whl"
```

---

### **3.3 Install ComfyUI-Manager**

Navigate to the custom nodes folder:

```
cd C:\comfy\ComfyUI\custom_nodes
```

Clone the manager:

```
git clone https://github.com/Comfy-Org/ComfyUI-Manager.git
```

---

## **4. Creating a BAT File to Launch ComfyUI**

The following batch script makes launching ComfyUI easier.

1. Copy the script below.
2. Save it as `start_comfyui.bat`.
3. Update the `COMFY_DIR` and `VENV_ACTIVATE` paths to match your setup.

```
@echo off

:: Path to your ComfyUI installation
set COMFY_DIR=C:\comfy\ComfyUI

:: Path to your Python virtual environment activation script
set VENV_ACTIVATE=C:\comfy\comfy\Scripts\activate.bat

echo [INFO] Activating Python virtual environment...
call "%VENV_ACTIVATE%"
if %ERRORLEVEL% neq 0 (
    echo [ERROR] Failed to activate virtual environment.
    pause
    exit /b 1
)

echo [INFO] Virtual environment activated.

:: Move to the ComfyUI directory
cd /d "%COMFY_DIR%"
echo [INFO] Working directory: "%CD%"

:: Start ComfyUI backend in a separate minimized window
echo [INFO] Starting ComfyUI backend...
start "" /MIN python main.py --use-sage-attention 

:: Wait for the backend to come up
:CHECK_SERVER
echo [INFO] Checking if server is fully up...
curl --silent --head http://127.0.0.1:8188/ | findstr /i "200 OK" >nul
if %ERRORLEVEL%==0 (
    echo [INFO] Server is online! Opening UI...
    start http://127.0.0.1:8188/
    goto FINISH
)

echo [INFO] Waiting for server to be online...
timeout /t 2 >nul
goto CHECK_SERVER

:FINISH
echo [INFO] ComfyUI started successfully. This window will close in 10 seconds...
timeout /t 10 >nul
exit
```

Run the launcher:

```
"C:\comfy\start_comfyui.bat"
```

If configured correctly, ComfyUI will start and the UI will open in your browser.

---

## **Reference Links**

**Python:** [https://www.python.org/downloads/release/python-31212](https://www.python.org/downloads/release/python-31212)

**Git:** [https://git-scm.com/install/windows](https://git-scm.com/install/windows)

**CUDA Toolkit:** [https://developer.nvidia.com/cuda-toolkit](https://developer.nvidia.com/cuda-toolkit)

**PyTorch CU130 builds:** [https://download.pytorch.org](https://download.pytorch.org)

**ComfyUI:** [https://github.com/comfyanonymous/ComfyUI](https://github.com/comfyanonymous/ComfyUI)

**Setup reference post:** [https://www.reddit.com/r/comfyui/comments/1n8v3zy/detailed_stepbystep_full_comfyui_with_sage/](https://www.reddit.com/r/comfyui/comments/1n8v3zy/detailed_stepbystep_full_comfyui_with_sage/)

**SAGE:** [https://github.com/woct0rdho/SageAttention/releases](https://github.com/woct0rdho/SageAttention/releases)

**ComfyUI Manager:** [https://github.com/Comfy-Org/ComfyUI-Manager](https://github.com/Comfy-Org/ComfyUI-Manager)

---
