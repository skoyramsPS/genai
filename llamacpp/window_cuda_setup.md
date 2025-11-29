# **Setting Up llama.cpp on Windows with CUDA (NVIDIA GPU)**

This guide provides a step-by-step walkthrough for building **llama.cpp** on Windows systems equipped with an NVIDIA GPU. The instructions assume the use of CUDA acceleration and Visual Studio build tools.

---

## **Table of Contents**

1. Assumptions
2. Prerequisites

   * Python
   * Git for Windows
   * Visual Studio 2019 VC Tools
   * vcpkg
   * Additional libraries
   * CUDA Toolkit 13.0
   * CUDA file integration with Visual Studio
   * CMake
3. llama.cpp Setup
4. Validating the Installation
5. Additional Information

---

## **1. Assumptions**

### **Winget Availability**

This guide assumes that **winget** is already installed on the system.

Verify with executing the following command in command prompt:
```
winget --version
```

* If a version such as `v1.12.350` is displayed, winget is available.
* If you see:
  `'winget' is not recognized as an internal or external command`
  then you must install winget first.
  Refer to Microsoft’s official documentation:
  [https://learn.microsoft.com/en-us/windows/package-manager/winget/](https://learn.microsoft.com/en-us/windows/package-manager/winget/)

---

## **2. Prerequisites**

### **2.1 Python**

Verify whether Python is installed, by executing the following command in command prompt:
```
python --version
```

or

```
python3 --version
```

* If both commands return “not recognized,” install Python 3.12 from the official site:
  [https://www.python.org/downloads/release/python-31212/](https://www.python.org/downloads/release/python-31212/)
* Ensure that **Add Python to PATH** is selected during installation.

---

### **2.2 Git for Windows**

Check whether Git is installed, by executing the following command in command prompt:

```
git --version
```

If not recognized, download and install Git from:
[https://git-scm.com/install/windows](https://git-scm.com/install/windows)

During installation, enable the option to **add Git to PATH**.

---

### **2.3 Visual Studio 2019 VC Tools**

Install required build tools using winget:

```
winget install Microsoft.VisualStudio.2019.BuildTools --override "--add Microsoft.VisualStudio.Workload.VCTools --includeRecommended"
```

The Visual Studio installer will open with the necessary components pre-selected. Proceed with installation.

---

### **2.4 vcpkg**

Clone vcpkg:

```
git clone https://github.com/microsoft/vcpkg.git C:\vcpkg
```

Navigate to the folder:

```
cd C:\vcpkg
```

Bootstrap vcpkg:

```
.\bootstrap-vcpkg.bat
```

Add vcpkg to your PATH:

```
setx PATH "%PATH%;C:\vcpkg"
```

---

### **2.5 Additional Libraries (zlib, curl, openssl)**

Install required packages using vcpkg:

```
vcpkg install zlib curl openssl --triplet x64-windows
```

---

### **2.6 CUDA Toolkit 13.0**

Download the CUDA Toolkit:
[https://developer.nvidia.com/cuda-toolkit](https://developer.nvidia.com/cuda-toolkit)

Install using the NVIDIA installer and complete the setup through the wizard.

---

### **2.7 Copy CUDA Integration Files into Visual Studio**

Navigate to:

```
C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v13.0\extras\visual_studio_integration\MSBuildExtensions
```

Copy all files from this folder into the following locations:

1.

```
C:\Program Files (x86)\Microsoft Visual Studio\2019\BuildTools\MSBuild\Microsoft\VC\v160\BuildCustomizations
```

2.

```
C:\Program Files (x86)\Microsoft Visual Studio\2019\BuildTools\MSBuild\Microsoft\VC\v150
```

---

### **2.8 CMake**

Install CMake using winget:

```
winget install kitware.CMake
```

---

## **3. llama.cpp Setup**

1. Create a directory where you want to place the llama.cpp repository.
2. Open Command Prompt, navigate to that directory, and clone the repository:

   ```
   git clone https://github.com/ggerganov/llama.cpp.git
   ```
3. Navigate to the project folder:

   ```
   cd llama.cpp
   ```
4. Create and enter the build directory:

   ```
   mkdir build
   cd build
   ```
5. Open **x64 Native Tools Command Prompt for VS 2019**.
6. Configure the project using CMake:

   ```
   cmake .. -G "Visual Studio 16 2019" -A x64 -DLLAMA_CUDA=ON -DGGML_VULKAN=OFF -DLLAMA_CURL=ON -DLLAMA_OPENSSL=ON -DCMAKE_TOOLCHAIN_FILE=C:/vcpkg/scripts/buildsystems/vcpkg.cmake -DCMAKE_GENERATOR_TOOLSET="cuda=C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v13.0"
   ```
7. Build the project:

   ```
   cmake --build . --config Release
   ```

Your llama.cpp installation is now ready.

---

## **4. Validating the Installation**

To run llama.cpp, download a small GGUF model such as:
[https://huggingface.co/unsloth/Qwen3-4B-GGUF](https://huggingface.co/unsloth/Qwen3-4B-GGUF)

Then test using:

```
llama-cli -m "Qwen3-4B-GGUF.gguf" -p "Tell me a story about a robot"
```

Replace the model file name with the full path of your downloaded GGUF file.

---

## **5. Additional Information**

### **Running llama.cpp as an API Server**

Example: model inference server

```
llama-server.exe --model "C:\GenAI\Models\Qwen3-Coder-30B-A3B-Instruct-Q3_K_M.gguf" --port 8088 --ctx-size 65536 --batch-size 4 --threads 4 --temp 0.7 --top-p 0.80 --top-k 20 --min-p 0.0 --presence-penalty 1.0
```

Example: embedding model server

```
llama-server.exe --model "C:\GenAI\Models\Qwen3-Embedding-0.6B-Q8_0.gguf" --port 8090 --ctx-size 32768 --batch-size 32 --threads 4
```




---
## References and Links

Winget: https://learn.microsoft.com/en-us/windows/package-manager/winget

Python: https://www.python.org/downloads/release/python-31212/

Git: https://git-scm.com/install/windows

NVIDIA Driver: https://www.nvidia.com/en-us/drivers/

NVIDIA CUDA Toolkit: https://developer.nvidia.com/cuda-toolkit

llama.cpp repo: https://github.com/ggml-org/llama.cpp

Visual Studio 2019 VC Tools installation: https://www.cognibuild.ai/installing-visual-studio-2019

Hugging Face for LLM Models: https://huggingface.co/
