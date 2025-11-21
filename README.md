# Python Environment Management Guide for macOS

## 📌 Introduction
This guide serves as a reference for managing Python installations on macOS. It explains how to clean up accidental installations and describes best practices for using **System Python**, **Homebrew Python**, and **Virtual Environments**.

---

## 🛠 Part 1: Emergency Cleanup (Restoring to Factory Settings)
If you accidentally installed multiple Python versions via Homebrew or messed up your PATH, follow these steps to remove them and revert to the macOS default.

### 1. Uninstall Homebrew Python Versions
Run the following command to remove Homebrew-managed Python versions (for example 3.11, 3.12, 3.13) and ignore dependency warnings:

```bash
brew uninstall --ignore-dependencies python@3.13 python@3.12 python@3.11
````

### 2. Remove Unused Dependencies (Leftovers)

After uninstalling Python, remove orphaned packages and clear the Homebrew cache:

```bash
brew autoremove
brew cleanup
```

### 3. Refresh Shell Path

Force the shell to forget previous locations of Python:

```bash
hash -r
```

### 4. Verify the Cleanup

Check whether the system is back to using the default macOS Python:

```bash
which python3
# Expected output: /usr/bin/python3

python3 --version
# Expected output: Python 3.9.x (or the macOS default version)
```

---

## 🧠 Part 2: The "Why and When" (Understanding Python Types)

There are three main types of Python environments on a Mac. Here’s how to treat each one:

### 1. System Python (`/usr/bin/python3`)

* **What is it?** The Python version that comes preinstalled with macOS.
* **Purpose:** Reserved for the operating system to run internal tasks.

> **RULE:** ⛔️ **NEVER install packages here.**
>
> **Why?** Installing libraries (e.g., `numpy`, `pandas`) requires `sudo` and can break macOS internal tools. Apple may also reset this directory during OS updates, which could remove your changes.

### 2. Homebrew Python (`/opt/homebrew/bin/python3`)

* **What is it?** A newer Python installed via Homebrew (e.g., `brew install python`).
* **Purpose:** Best used for global command-line tools (like `youtube-dl`, `ansible`, `aws-cli`).

> **RULE:** ✅ **Use it for CLI tools**, ❌ **do not use it for project development.**
>
> **Why?** Installing project libraries globally causes dependency conflicts between projects (Dependency Hell).

### 3. Virtual Environments (`venv` / `conda`) 🏆 (Recommended)

* **What is it?** An isolated folder containing a specific Python interpreter and libraries for a single project.
* **Purpose:** Use for all development work (AI, data science, web dev, crypto bots, etc.).

> **RULE:** ✅ **ALWAYS use virtual environments for your projects.**
>
> **Why?**
>
> * Project A can use `tensorflow 2.0`.
> * Project B can use `tensorflow 1.0`.
> * Deleting a project is as simple as removing its venv directory (`rm -rf venv`).
> * Keeps your Mac clean and prevents global conflicts.

---

## 🚀 Part 3: The Correct Workflow (Best Practice)

When starting a new project (for example, a Cardano ADA bot or an AI project), follow this workflow:

### Step 1: Create a project folder

```bash
mkdir my_crypto_project
cd my_crypto_project
```

### Step 2: Create a Virtual Environment (one-time)

This creates a folder named `venv` (or any name you prefer) inside your project:

```bash
# Use Python's built-in venv module
python3 -m venv venv
```

### Step 3: Activate the Environment (every time you work)

This makes the shell use the project's Python instead of the system Python:

```bash
source venv/bin/activate
```

You will see the prompt change to show:

```
(venv)
```

### Step 4: Install Libraries safely

Now you can install any libraries without affecting the rest of your system:

```bash
pip install numpy pandas
```

### Step 5: Deactivate when done

```bash
deactivate
```

---

## 🔍 Troubleshooting Commands

**Check where Python is running from:**

```bash
which python3
```

**List installed libraries:**

```bash
pip3 list
```

**Inspect Python’s search paths (where it looks for libraries):**

```
python3 -m site
```

---


