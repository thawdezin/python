Python Environment Management Guide for macOS📌 IntroductionThis guide serves as a reference for managing Python installations on macOS. It covers how to clean up accidental installations and explains the best practices for using System Python vs. Homebrew Python vs. Virtual Environments.🛠 Part 1: Emergency Cleanup (Restoring to Factory Settings)If you accidentally installed multiple Python versions via Homebrew or messed up your path, follow these steps to remove them and revert to the macOS default.1. Uninstall Homebrew Python VersionsRun the following command to remove all Homebrew-managed Python versions (e.g., 3.11, 3.12, 3.13) while ignoring dependency warnings.brew uninstall --ignore-dependencies python@3.13 python@3.12 python@3.11
2. Remove Unused Dependencies (Leftovers)After uninstalling Python, remove orphaned packages and clear the Homebrew cache.brew autoremove
brew cleanup
3. Refresh Shell PathForce the terminal to forget the old locations of Python.hash -r
4. Verify the CleanupCheck if the system is back to using the default macOS Python.which python3
# Expected Output: /usr/bin/python3

python3 --version
# Expected Output: Python 3.9.6 (or similar default macOS version)
🧠 Part 2: The "Why and When" (Understanding Python Types)There are three main types of Python environments on a Mac. Here is how to treat them:1. System Python (/usr/bin/python3)What is it? The Python version that comes pre-installed with macOS.Purpose: It exists solely for the operating system to run its own tasks.RULE: ⛔️ NEVER install packages here.Why? Installing libraries (like numpy or pandas) here requires sudo and can break macOS internal tools. Apple may also reset this directory during OS updates, deleting your work.2. Homebrew Python (/opt/homebrew/bin/python3)What is it? A newer version of Python installed via brew install python.Purpose: To run global command-line tools (e.g., youtube-dl, ansible, aws-cli).RULE: ✅ Use it for tools, but ❌ avoid using it for coding projects.Why? If you install project libraries here, they will conflict with each other as you start working on different projects (Dependency Hell).3. Virtual Environments (venv / conda) 🏆 (Recommended)What is it? An isolated folder containing a specific Python version and libraries for one specific project.Purpose: All Development (AI, Data Science, Web Dev, Crypto bots).RULE: ✅ ALWAYS use this for your projects.Why?Project A can use tensorflow 2.0.Project B can use tensorflow 1.0.Deleting the project is as simple as deleting the folder (rm -rf venv). It keeps your Mac clean.🚀 Part 3: The Correct Workflow (Best Practice)Whenever you start a new project (e.g., a Cardano ADA bot or AI project), follow this workflow:Step 1: Create a project foldermkdir my_crypto_project
cd my_crypto_project
Step 2: Create a Virtual Environment (One time)This creates a folder named venv (or any name you like) inside your project.# Uses the built-in venv module
python3 -m venv venv
Step 3: Activate the Environment (Every time you work)This tells your terminal to use the project's Python, not the System Python.source venv/bin/activate
(You will see (venv) appear in your terminal prompt)Step 4: Install Libraries safelyNow you can install anything without affecting the rest of your Mac.pip install numpy pandas
Step 5: Deactivate when donedeactivate
🔍 Troubleshooting CommandsCheck where Python is running from:which python3
Check installed libraries:pip3 list
Check search paths (where Python looks for libs):python3 -m site
