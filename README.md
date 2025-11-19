📄 README – Cloning & Opening This Project Correctly
📥 1. Clone the Repository (Important: Include Submodules)

This project uses multiple sub-repositories (submodules).
To ensure all application code is downloaded, you must clone with submodules enabled:

```angular2html
git clone --recursive https://github.com/York-Solutions-B2E/tsg-TheAvengers-final-gt-mf.git

```
If you already cloned without --recursive, run:
```angular2html
git submodule update --init --recursive
```
This pulls in:

feedback-api/

feedback-analytics-consumer/

frontend-feedback-ui/

Each of these is a standalone application referenced by the master repository.

🗂 2. Opening the Project (IntelliJ / VS Code)

To avoid issues with Git submodules being detected as empty folders:

✅ Recommended:

Open the project through the menu:

IntelliJ IDEA
```angular2html
File → Open… → Select the project folder (tsg-TheAvengers-final-gt-mf)
```

VS Code
```angular2html
File → Open Folder… → Select the project folder
```

❌ Avoid:

Dragging the folder from Finder into IntelliJ or VS Code

Using “Open Recent” if the project structure changed

Using IntelliJ’s built-in Git clone popup (it does not always initialize submodules)

Opening the project through the menu ensures the IDE correctly detects:

the Git repository

the submodules

Maven projects

nested Spring Boot applications

the frontend application structure

🔍 3. Verifying the Submodules Downloaded

After cloning, each service folder should contain code (not empty directories).
You can verify them by running:
```angular2html
ls feedback-api
ls feedback-analytics-consumer
ls frontend-feedback-ui
```