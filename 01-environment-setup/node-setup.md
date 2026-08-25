# Installing Node.js, npm, and Bun
Some projects use JavaScript/TypeScript tooling (web frontends, notebook extensions, build tools), which needs a Node.js runtime. This guide covers installing node along with npm, the package manager that ships with it, and Bun as an optional faster alternative.

## Steps
1. Install node
    - Linux
        1. We will use [nvm](https://github.com/nvm-sh/nvm) (node version manager), which lets you install and switch between multiple node versions per project, similar to pyenv for python
        2. Run the following in your terminal
            ``` bash
            curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash
            ```
            - check the [nvm releases page](https://github.com/nvm-sh/nvm/releases) for the latest version and update `v0.40.3` in the url if a newer one is available
        3. Restart your terminal, or reload your shell config
            ``` bash
            source ~/.bashrc
            ```
            - if you use zsh, reload `~/.zshrc` instead
        4. Install the latest long term support (LTS) release of node
            ```
            nvm install --lts
            nvm use --lts
            ```
        5. To install a different version later, run `nvm install $version` then `nvm use $version`. Replace `$version` with the version you need, for example `22`
    - macOS
        1. Download the LTS installer from the [nodejs download page](https://nodejs.org/en/download) and run it
        2. Alternatively, install it with homebrew
            ``` bash
            brew install node
            ```
        3. Verify the installation
            ``` bash
            node -v
            npm -v
            ```
    - Windows
        1. Download the LTS installer from the [nodejs download page](https://nodejs.org/en/download) and run it
        2. Alternatively, install it with winget
            ``` powershell
            winget install OpenJS.NodeJS.LTS
            ```
        3. Open a new terminal and verify the installation
            ``` powershell
            node -v
            npm -v
            ```
2. (optional) Install Bun
    - [Bun](https://bun.sh/) is an all-in-one runtime, package manager, and test runner that is largely drop-in compatible with node and much faster than npm. It is not required, but some projects use it instead of npm
    - macOS/Linux
        ``` bash
        curl -fsSL https://bun.sh/install | bash
        ```
    - Windows (powershell)
        ``` powershell
        powershell -c "irm bun.sh/install.ps1|iex"
        ```
    - Alternatively, install it through npm
        ```
        npm install -g bun
        ```
    - Verify the installation
        ```
        bun --version
        ```

## Additional debugging
* If `npm install` fails behind a corporate proxy or SSL inspection (e.g. zscaler) with certificate errors, the same troubleshooting from the [vs-codium-setup](vs-codium-setup.md#additional-debugging) guide applies. You can point npm at your certificate bundle
    ```
    npm config set cafile $path-to-cacert.pem
    ```
    - Replace `$path-to-cacert.pem` with the path to the certificate bundle, see the SSL section of vs-codium-setup for where to find it
