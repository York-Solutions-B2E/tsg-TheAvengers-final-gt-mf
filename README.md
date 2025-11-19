# Cloning & Opening This Project Correctly

## 1. Clone the Repository (Important: Includes Submodules)

This project uses Git submodules to reference:

- `feedback-api/`
- `feedback-analytics-consumer/`
- `frontend-feedback-ui/`

To ensure the full project is downloaded, clone **with submodules enabled**:

```angular2html
git clone --recursive https://github.com/York-Solutions-B2E/tsg-TheAvengers-final-gt-mf.git

```

If you already cloned it without --recursive, run:
```angular2html
git submodule update --init --recursive
```

## 2. Opening the Project (IntelliJ / VS Code)
To avoid issues where submodule folders appear empty, always open the project using the IDE's menu system.

IntelliJ IDEA
```angular2html
File → Open… → Select the project folder (tsg-TheAvengers-final-gt-mf)
```

VS Code
```angular2html
File → Open Folder… → Select the project folder
```

Avoid:

Dragging the folder from Finder into IntelliJ or VS Code

Using “Open Recent”

Using IntelliJ’s built-in Git clone dialog (it does not initialize submodules consistently)