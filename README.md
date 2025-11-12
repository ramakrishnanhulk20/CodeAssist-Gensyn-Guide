# Gensyn CodeAssist Installation Guide for Windows 11

A detailed guide for setting up Gensyn's CodeAssist on Windows 11, including common errors, solutions, and performance benchmarks.

## About This Guide

This guide documents the complete installation process of Gensyn CodeAssist on Windows 11, including errors I encountered and their solutions. CodeAssist is a private, local AI coding assistant that learns from your coding style and helps you practice programming problems.

## What is CodeAssist?

CodeAssist is developed by Gensyn and acts as an AI programming assistant that:
* Writes code directly in your editor as you work
* Learns from every keystroke you make
* Trains a personalized model based on your coding style
* Works completely locally and privately
* Supports LeetCode problem practice

Unlike traditional code assistants, CodeAssist adapts to your habits over time, acting more like an apprentice learning from your craft.

## System Specifications Used

This installation was tested on the following system:
* **Processor**: Intel Core i5 11th Gen H series
* **Graphics**: NVIDIA GeForce RTX 3050 Ti
* **Operating System**: Windows 11
* **Internet Speed**: 50 Mbps

## Performance Benchmarks

Based on the above system specifications:

**Initial Setup Times:**
* Docker container build (first container): approximately 15 minutes
* Total container setup (all 5 containers): approximately 20 to 25 minutes
* Model training (3 episodes): 5 to 10 minutes
* Complete first time setup: 40 to 50 minutes total

**Data Requirements:**
* Total Docker images downloaded: approximately 15 GB
* Ensure you have at least 20 GB free disk space

## Prerequisites

Before starting, ensure you have:
1. Windows 10/11 (Pro, Enterprise, Education, or Home with WSL 2)
2. At least 20 GB free disk space (preferably on a drive with more space)
3. Administrator access to install software
4. Stable internet connection
5. HuggingFace account with a Write access token

## Installation Steps

### Step 1: Install Python

1. Check if Python is already installed by opening PowerShell and running:
```powershell
python --version
```

2. If not installed or version is below 3.10, download Python 3.11 or higher from the official website
3. During installation, ensure you check "Add Python to PATH"
4. Verify installation:
```powershell
python --version
```

Expected output: Python 3.11.x or higher

### Step 2: Install Git

1. Check if Git is installed:
```powershell
git --version
```

2. If not installed, download Git for Windows from the official website
3. Install with default settings
4. Verify installation:
```powershell
git --version
```

### Step 3: Install UV Package Manager

UV is required to manage Python dependencies for CodeAssist.

Install UV using PowerShell:
```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

After installation:
1. Close and reopen PowerShell
2. Verify installation:
```powershell
uv --version
```

### Step 4: Install Docker Desktop

Docker is required to run CodeAssist containers.

1. Download Docker Desktop for Windows from the official Docker website
2. Run the installer (file will be named Docker Desktop Installer.exe)
3. Use default settings (it should use WSL 2 backend)
4. **Important**: Restart your computer after installation
5. After restart, open Docker Desktop application
6. Verify Docker is running:
```powershell
docker --version
```

**Critical: Configure Docker Storage Location**

If your C: drive has limited space, you must move Docker's data directory:

1. Open Docker Desktop
2. Go to Settings (gear icon)
3. Navigate to Resources → Advanced
4. Change the disk image location from C: drive to another drive with more space (e.g., D: drive)
5. Alternatively, create/edit the daemon configuration file at `C:\ProgramData\Docker\config\daemon.json`:
```json
{
  "data-root": "D:\\DockerData"
}
```
6. Restart Docker Desktop completely

### Step 5: Get HuggingFace Token

1. Go to https://huggingface.co/settings/tokens
2. Sign up or log in to your account
3. Click "New token"
4. Give it a name (e.g., "CodeAssist")
5. **Important**: Select "Write" access level
6. Copy the token and save it securely

### Step 6: Clone the Repository

1. Create a directory for the project:
```powershell
mkdir D:\CodeAssist
cd D:\CodeAssist
```

2. Clone the repository:
```powershell
git clone https://github.com/gensyn-ai/codeassist.git
cd codeassist
```

### Step 7: Run CodeAssist

1. Start CodeAssist:
```powershell
uv run run.py
```

2. When prompted, paste your HuggingFace token
   * Note: The token will not be visible as you type (this is normal for security)

<img width="1920" height="986" alt="Screenshot (39)" src="https://github.com/user-attachments/assets/5245e855-4a80-4d9a-a030-77425ac58d8c" />

3. Wait for all containers to start. This will:
   * Create a virtual environment at `.venv`
   * Install all dependencies
   * Pull and build Docker images (this takes 20 to 25 minutes on first run)
   * Start 5 containers: Ollama, Policy Models, Web UI, State Service, Solution Tester

<img width="1920" height="1017" alt="Screenshot (41)" src="https://github.com/user-attachments/assets/1b26037d-55ad-4908-87f0-c204400b2b37" />
  
4. Once you see "CodeAssist Started", your browser should automatically open to http://localhost:3000

<img width="1920" height="965" alt="Screenshot (44)" src="https://github.com/user-attachments/assets/78bffa33-3b36-47d0-b07c-1f37e9bb30e1" />

5. If the browser doesn't open automatically, manually navigate to:
```
http://localhost:3000
```

### Step 8: Use CodeAssist

1. Log in using email (one time passcode) or Google account

<img width="1920" height="962" alt="Screenshot (45)" src="https://github.com/user-attachments/assets/bc9cce8a-c5a8-462c-adab-b78688ce7be4" />

2. Select a problem difficulty (Easy, Medium, or Hard) from the sidebar
3. Start coding in the editor.( Delete all the existing code and start fresh if you get error for the problem )
4. The assistant will begin suggesting code as you type
5. Use `Shift+Space` to pause/unpause the assistant or you can just press the pause button at the botton of the editor.

**Tips for Best Results:**
* Start from fresh to keep your cursor near the section you're working on
* Be patient and let the assistant suggest code before immediately deleting
* Work on multiple different problems to improve training diversity
* The assistant improves after 4 to 5 training episodes

### Step 9: Training Your Model

To train your personalized model:

1. After solving one or more problems, return to the terminal where CodeAssist is running
2. Press `Ctrl+C` to stop recording and start training

<img width="1920" height="1011" alt="Screenshot (46)" src="https://github.com/user-attachments/assets/65481915-6e1d-478a-ae95-4155d76faf8b" />

3. Training will:
   * Compare your edits to the assistant's suggestions
   * Calculate rewards based on your interactions
   * Update your local model checkpoint
   * Save models to `persistent-data/trainer/models`
   * Upload trained models to your HuggingFace account

Training typically takes 5 to 10 minutes depending on your system.

<img width="1920" height="1007" alt="Screenshot (50)" src="https://github.com/user-attachments/assets/26ba8596-7add-4548-a72a-9870e8823d6a" />

### Step 9: Training Your Model

You can see score on the gensyn's dashboard at https://dashboard.gensyn.ai/?application=CodeAssist

<img width="1920" height="894" alt="Screenshot (52)" src="https://github.com/user-attachments/assets/d26f939d-2bca-4224-89ed-f18732a57f86" />

## Common Errors and Solutions

### Error 1: Multiple Top Level Packages

**Error Message:**
```
Multiple top-level packages discovered in a flat-layout: ['datasets', 'ca_alchemy', 'policy_models']
```

**Cause:** 
Attempting to install with `pip install -e .` instead of using UV package manager.

**Solution:**
Always use `uv run run.py` instead of pip install. The project requires UV for dependency management.

### Error 2: Docker Build Stuck at 1/5

**Symptoms:**
* Container setup progress bar stays at "1/5" for more than 10 minutes
* Docker appears to be doing nothing

**Cause:**
Insufficient disk space on C: drive where Docker stores images by default.

**Solution:**
1. Stop the CodeAssist process with `Ctrl+C`
2. Move Docker's data directory to a drive with more space (see Step 4 above)
3. Clean up existing Docker data:
```powershell
docker system prune -a
```
4. Restart CodeAssist with `uv run run.py`

### Error 3: HuggingFace Upload Failed (401 Unauthorized)

**Error Message:**
```
401 Client Error: Unauthorized for url: https://huggingface.co/api/whoami-v2
Invalid user token
```

**Cause:**
HuggingFace token is invalid, expired, or doesn't have Write permissions.

**Solution:**
1. Generate a new token with Write access from https://huggingface.co/settings/tokens
2. Set the token as an environment variable:
```powershell
$env:HF_TOKEN = "your_new_token_here"
```
3. Run CodeAssist again:
```powershell
uv run run.py
```

Note: Models are saved locally even if HuggingFace upload fails, so your training is not lost.

### Warning: chmod Not Recognized

**Warning Message:**
```
'chmod' is not recognized as an internal or external command
```

**Cause:**
The script attempts to use a Linux command on Windows.

**Impact:**
This warning can be safely ignored. It does not affect functionality.

### Warning: Virtual Environment Mismatch

**Warning Message:**
```
warning: `VIRTUAL_ENV=venv` does not match the project environment path `.venv`
```

**Cause:**
UV creates its own virtual environment at `.venv` but detects an existing `venv`.

**Impact:**
This warning can be safely ignored. UV will use its own `.venv` directory.

## Troubleshooting

### Containers Won't Start

If you see "Container is unhealthy" errors:
1. Check Docker logs:
```powershell
docker logs <container-name>
```
2. Ensure Docker Desktop is running
3. Verify you have sufficient disk space
4. Try restarting Docker Desktop

### Port Already in Use

If you see "port is already allocated" errors:
1. Stop other services using the same port
2. Or run CodeAssist on a different port:
```powershell
uv run run.py --port 3001
```

Note: Avoid ports 8000, 8080, 8001, 8008, 3003, and 11434 as they are reserved for CodeAssist services.

### Cannot Connect to Docker Daemon

If you see connection errors:
1. Ensure Docker Desktop is running
2. Check that you have permission to access Docker
3. Try restarting Docker Desktop
4. On Windows, ensure WSL 2 is properly installed

## File Structure After Installation

After successful installation, your directory will contain:

```
codeassist/
├── persistent-data/
│   ├── trainer/models/          (your trained models)
│   ├── state-service/episodes/  (recorded episodes)
│   └── auth/                    (authentication data)
├── logs/_training_artifacts/    (training logs and checkpoints)
├── .venv/                       (Python virtual environment)
└── [other project files]
```

## Additional Resources

* Official Documentation: https://docs.gensyn.ai/testnet/codeassist
* Tutorial: https://docs.gensyn.ai/testnet/codeassist/using-codeassist
* Leaderboard: https://dashboard.gensyn.ai/?application=CodeAssist
* GitHub Repository: https://github.com/gensyn-ai/codeassist

Made with ❤️ by Ram

