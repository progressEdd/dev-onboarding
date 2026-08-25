# Installing Node.js, npm, and Bun
Some projects use JavaScript/TypeScript tooling (web frontends, notebook extensions, build tools), which needs a Node.js runtime. This guide covers installing node with nvm (recommended, the node equivalent of pyenv), which also installs npm, and Bun as an optional faster alternative.

## Steps
1. Install nvm
    - nvm lets you install and switch between multiple node versions per project, similar to pyenv for python
    - macOS/Linux
        1. Run the following in your terminal
            ``` bash
            curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash
            ```
            - check the [nvm releases page](https://github.com/nvm-sh/nvm/releases) for the latest version and update `v0.40.3` in the url if a newer one is available
        2. Restart your terminal, or reload your shell config
            ``` bash
            source ~/.bashrc
            ```
            - if you use zsh, reload `~/.zshrc` instead
        3. Verify the installation
            ``` bash
            nvm --version
            ```
    - Windows
        1. Windows uses a separate project, [nvm-windows](https://github.com/coreybutler/nvm-windows). Download and run the latest `nvm-setup.exe` from the [releases page](https://github.com/coreybutler/nvm-windows/releases)
        2. Verify the installation in a new terminal
            ``` powershell
            nvm version
            ```
2. Install node
    1. Run the following command to install the latest long term support (LTS) release
        ```
        nvm install --lts
        ```
    2. Tell nvm to use it
        ```
        nvm use --lts
        ```
    3. Verify node and npm (npm installs alongside node)
        ```
        node -v
        npm -v
        ```
    - To install a different version later, run `nvm install $version` then `nvm use $version`. Replace `$version` with the version you need, for example `22`
3. (optional) Install Bun
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
