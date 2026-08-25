# dev-onboarding
## Background
Welcome to this repository! I created this repo initially to recall how I configured my own development environment. As I onboard junior developers to python GenAI projects, I realized that the pages I created would also be helpful to refer people to. The intended audience has a beginner to intermediate python development experience.

Feel free to report any issues on the [issues tab](https://github.com/progressEdd/dev-onboarding/issues)

## Guides
The folders are numbered by the suggested reading order for new developers. Images and other supporting files live in [00-supporting-files](00-supporting-files).

### 01-environment-setup
Setting up your editor and machine before writing any code.
- [vs-codium-setup.md](01-environment-setup/vs-codium-setup.md) — Installing VSCode/VSCodium, its extensions, and importing the `vs-dev` code profile
- [multiple-ssh.md](01-environment-setup/multiple-ssh.md) — Configuring multiple SSH keys for separate Git accounts (e.g. personal and work)

### 02-dev-workflows
Day-to-day workflows for contributing code and taking notes.
- [intro-to-git.md](02-dev-workflows/intro-to-git.md) — Git fundamentals: the common workflow, terminology, and working with worktrees
- [intro-to-notes.md](02-dev-workflows/intro-to-notes.md) — Taking notes with Foam and markdown in VS Code/Codium

### 03-python-environments
Python environment setup, assumes your editor is already installed.
- [python-virtual-environments.md](03-python-environments/python-virtual-environments.md) — Creating and managing python environments with UV (recommended), miniforge/conda, and poetry
