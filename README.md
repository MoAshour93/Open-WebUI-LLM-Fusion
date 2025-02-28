# Open WebUI Setup Guide

![Open WebUI Banner](https://raw.githubusercontent.com/open-webui/open-webui/main/images/banner.png)

A comprehensive guide to setting up Open WebUI with local models, cloud-based models, and image generation capabilities.

## 📑 Overview

This guide helps you set up Open WebUI, a user-friendly interface that allows you to:
- Chat with local and online Large Language Models (LLMs)
- Generate images using AI models
- Query and interact with various document types

## 📋 Table of Contents

1. [Requirements](#requirements)
2. [Installation Guide](#installation-guide)
   - [Python Setup](#1-python)
   - [HomeBrew Installation](#2-homebrew)
   - [Git Installation](#3-git-scm)
   - [Open WebUI Setup](#4-open-webui)
   - [Open WebUI Pipelines](#5-open-webui-pipelines)
   - [ComfyUI Setup](#6-comfyui)
   - [Stable Diffusion WebUI](#7-stable-diffusion-webui)
3. [Configuration](#configuration)
   - [Adding Cloud-Based Models](#adding-anthropic-pipeline-to-open-webui)
   - [Setting Up Image Generation](#activate-image-generation-within-open-webui-using-comfy-ui-api)
4. [Testing & Deployment](#deployment-within-open-webui)
5. [Resources](#resources)

## Requirements

Before starting, ensure you have:
- A computer running macOS (Windows steps are similar but not identical)
- Internet connection for downloading software and models
- Basic familiarity with terminal/command line

## Installation Guide

### 1. Python

Python is essential for AI and ML applications.

**Installation Steps:**
1. Download Python 3.11 or later from [python.org/downloads](https://www.python.org/downloads)
2. Add Python to your PATH:
   ```bash
   # Check Python location
   which python3
   echo $PATH
   
   # Find actual Python path
   ls -la $(which python3)
   
   # Add to PATH (for Zsh - default on newer macOS)
   echo 'export PATH="/path/to/python/bin:$PATH"' >> ~/.zshrc
   source ~/.zshrc
   
   # Verify installation
   python3 --version
   ```

3. Install PIP (Python package manager):
   ```bash
   python3 -m ensurepip --upgrade
   python3 -m pip --version
   ```

### 2. HomeBrew

HomeBrew is a package manager for macOS.

**Installation:**
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Visit [brew.sh](https://brew.sh) for more information.

### 3. Git-SCM

Git allows you to clone repositories from GitHub.

**Installation:**
```bash
brew install git
```

Learn more at [git-scm.com](https://git-scm.com).

### 4. Open WebUI

An extensible AI platform designed to operate offline.

**Installation:**
```bash
pip install open-webui
```

**Running Open WebUI:**
```bash
open-webui serve
```

Access Open WebUI in your browser at: [http://localhost:8080](http://localhost:8080)

Find more details at [github.com/open-webui/open-webui](https://github.com/open-webui/open-webui).

### 5. Open WebUI Pipelines

Pipelines add modular workflows to Open WebUI.

**Installation:**
```bash
# Find Open WebUI installation folder
pip show open-webui

# Clone the pipelines repository
git clone https://github.com/open-webui/pipelines.git

# Navigate to the pipelines folder
cd pipelines

# Install dependencies
pip install -r requirements.txt

# Start pipelines
sh ./start.sh
```

Pipelines will run at [http://localhost:9090](http://localhost:9090).

### 6. ComfyUI

ComfyUI generates images, videos, and more through node-based workflows.

**Installation Steps:**
1. Download from [comfy.org](https://www.comfy.org/) for your system (Windows/macOS)
2. Download and install models:

   **For Flux models:**
   - Download [Flux Developer](https://huggingface.co/black-forest-labs/FLUX.1-dev) or [Flux Schnell](https://huggingface.co/black-forest-labs/FLUX.1-schnell)
   - Download [VAE model](https://huggingface.co/black-forest-labs/FLUX.1-schnell/blob/main/ae.safetensors)
   - Download [text encoders](https://huggingface.co/comfyanonymous/flux_text_encoders/tree/main) (t5xxl_fp16 & clip_l)
   - Download [workflows](https://comfyanonymous.github.io/ComfyUI_examples/flux/)

   **For Stable Diffusion models:**
   - Download [SD 3.5 Large](https://huggingface.co/stabilityai/stable-diffusion-3.5-large) or [SD 3.5 Large Turbo](https://huggingface.co/stabilityai/stable-diffusion-3.5-large-turbo)
   - Download text encoders: [clip_g](https://huggingface.co/stabilityai/stable-diffusion-3-medium/resolve/main/text_encoders/clip_g.safetensors), [clip_l](https://huggingface.co/stabilityai/stable-diffusion-3-medium/resolve/main/text_encoders/clip_l.safetensors), [t5xxlf16](https://huggingface.co/stabilityai/stable-diffusion-3-medium/resolve/main/text_encoders/t5xxl_fp16.safetensors)
   - Download [example workflows](https://huggingface.co/Comfy-Org/stable-diffusion-3.5-fp8/blob/main/sd3.5-t2i-fp16-workflow.json)

3. **Enable API JSON export:**
   - Go to settings (gear icon)
   - Enable "Enable dev mode options (Save APIs..etc)"
   - Use "Workflow" > "Export (API)" to save workflows

### 7. Stable Diffusion WebUI

**New Installation:**
```bash
# Install dependencies
brew install cmake protobuf rust python@3.11 git wget

# Clone repository
git clone https://github.com/AUTOMATIC1111/stable-diffusion-webui

# Download models from huggingface.co/stabilityai
# Place models in stable-diffusion-webui/models/Stable-diffusion

# Launch WebUI
cd stable-diffusion-webui
./webui.sh
```

**Existing Installation:**
Delete `run_webui_mac.sh` and `repositories` folder, then run:
```bash
git pull
./webui.sh
```

## Configuration

### Adding Anthropic Pipeline to Open WebUI

1. Start Open WebUI:
   ```bash
   open-webui serve
   ```

2. Start Pipelines:
   ```bash
   cd pipelines
   sh ./start.sh
   ```

3. In Open WebUI:
   - Go to profile icon → Admin Panel → Settings → Connections
   - Add new connection:
     - URL: http://localhost:9099
     - Key: 0p3n-w3bu!
   - Click refresh to verify connection, then save

4. Navigate to Settings → Pipelines:
   - Upload provider files from `pipelines/examples/pipelines/providers/`
   - Enter your API key for the chosen cloud provider

### Activate Image Generation within Open WebUI using ComfyUI API

1. Start ComfyUI
2. In Open WebUI:
   - Go to Admin Panel → Settings → Images
   - Enable "Image Generation"
   - Set ComfyUI base URL: http://127.0.0.1:8000
   - Upload your exported Workflow API JSON file
   - Configure nodes for:
     - Prompt
     - Model
     - Width
     - Height
     - Steps
     - Seed
   - Select model and image settings

## Deployment within Open WebUI

Now you can:
1. Start a new chat in Open WebUI
2. Select one of your configured models
3. Use the LLM to write detailed image prompts
4. Click the image icon under the LLM response to generate images via ComfyUI

## Resources

Helpful tutorial videos:
- [Install Stable Diffusion on Comfy UI](https://www.youtube.com/watch?v=3-pQqJJgqy8)
- [Install Flux on Comfy UI](https://www.youtube.com/watch?v=ImWHS5Ux36E)
- [Run Comfy UI Workflows in Open WebUI](https://www.youtube.com/watch?v=t68_mYLnSG4)
