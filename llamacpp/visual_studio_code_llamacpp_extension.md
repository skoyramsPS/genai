# Setting Up the **llama.cpp** Extension for Autocompletion in Visual Studio Code

This guide walks you through preparing Visual Studio Code and configuring it to work with an existing **llama.cpp** build.

## **Table of Contents**

1. Pre-requisites
2. Configuring the llama.cpp VS Code Extension

---

## **1. Pre-requisites**

### **1.1 Visual Studio Code**

Ensure Visual Studio Code is installed on your system.
You may download it from the official website:
[https://code.visualstudio.com/download](https://code.visualstudio.com/download)

### **1.2 llama.cpp Build and Setup**

Make sure you already have a working installation of **[llama.cpp](https://github.com/ggml-org/llama.cpp)**.
If you haven’t set it up yet, follow this [guide](window_cuda_setup.md) first.

---

## **2. Configuring the llama.cpp VS Code Extension**

### **2.1 Install the Extension**

1. Launch Visual Studio Code.
2. Open the Extensions panel (or press **Ctrl + Shift + X**).
3. Search for **llama-vscode**.
4. Install the extension.
5. When prompted to install **llama.cpp**, select **“No”** since you already have a build prepared.

---

### **2.2 Start the llama.cpp Server**

Before configuring the extension, start your local llama.cpp server.

Open **Command Prompt** or any terminal and run a model compatible with your system.

Example command to start an LLM model on port **8088**:

```
llama-server.exe --model "C:\MODEL.gguf" --port 8088
```

Example command to start an embedding model on port **8090**:

```
llama-server.exe --model "C:\Embedding.gguf" --port 8090
```

You can download suitable models from **Hugging Face**:
[https://huggingface.co/](https://huggingface.co/)

Once the server is running, it will display the endpoint URLs.
Based on the commands above, the URLs will be:

* **LLM endpoint (port 8088):**

  ```
  http://127.0.0.1:8088
  ```

* **Embedding endpoint (port 8090):**

  ```
  http://localhost:8090
  ```

---

### **2.3 Configure the llama-vscode Extension**

1. Open VS Code settings (click the **gear icon** in the lower-left corner).

2. Search for:

   ```
   llama.vscode
   ```

3. In the extension settings, update the fields:

   * **Llama-vscode: Endpoint**
   * **Llama-vscode: Endpoint_chat**
   * **Llama-vscode: Endpoint_tools**

   Set all of the above to:

   ```
   http://127.0.0.1:8088
   ```

   * **Llama-vscode: Endpoint_embeddings**

    ```
    http://localhost:8090
    ```

4. Disable the option:
   **Llama-vscode: Ask_install_llamacpp**

After completing the configuration, **restart VS Code**.
Autocompletion and chat features will now operate using your local **llama.cpp** setup.

---
