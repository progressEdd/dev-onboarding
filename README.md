# dev-onboarding
## Background
Welcome to this repository! I created this repo initially to recall how I configured my own development environment. As I onboard junior developers to python GenAI projects, I realized that the pages I created would also be helpful to refer people to. The intended audience has a beginner to intermediate python development experience.

Feel free to report any issues on the [issues tab](https://github.com/progressEdd/dev-onboarding/issues)

## Guides
The folders are numbered by the suggested reading order. [vs-codium-setup](01-environment-setup/vs-codium-setup.md) is the entry point for the whole setup and links out to the other guides partway through, so expect to jump between 01 and 03. Images and other supporting files live in [00-supporting-files](00-supporting-files).

### 01-environment-setup
Setting up your editor, machine, and language runtimes before writing any code.
- [vs-codium-setup.md](01-environment-setup/vs-codium-setup.md) — Installing VSCode/VSCodium, its extensions, and importing the `vs-dev` code profile. Entry point for the whole setup
- [node-setup.md](01-environment-setup/node-setup.md) — Installing Node.js and npm (nvm on Linux, direct install on mac/windows), plus Bun as an optional faster alternative
- [multiple-ssh.md](01-environment-setup/multiple-ssh.md) — Configuring multiple SSH keys for separate Git accounts (e.g. personal and work)

### 02-dev-workflows
Day-to-day workflows for contributing code and taking notes.
- [intro-to-git.md](02-dev-workflows/intro-to-git.md) — Git fundamentals: the common workflow, terminology, and working with worktrees
- [intro-to-notes.md](02-dev-workflows/intro-to-notes.md) — Taking notes with Foam and markdown in VS Code/Codium

### 03-python-environments
Python environment setup with UV (recommended), miniforge/conda, and poetry. Followed midway through vs-codium-setup, before the editor configuration steps.
- [python-virtual-environments.md](03-python-environments/python-virtual-environments.md) — Creating and managing python environments
