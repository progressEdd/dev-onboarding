# Installing Node.js, npm, and Bun
Some projects use JavaScript/TypeScript tooling (web frontends, notebook extensions, build tools), which needs a Node.js runtime. This guide covers installing node directly along with npm, the package manager that ships with it, and Bun as an optional faster alternative.

## Steps
1. Install node
    - Windows
        1. Download the LTS (long term support) installer from the [nodejs download page](https://nodejs.org/en/download) and run it
        2. Alternatively, install it with winget
            ``` powershell
            winget install OpenJS.NodeJS.LTS
            ```
        3. Open a new terminal and verify the installation, npm installs alongside node
            ``` powershell
            node -v
            npm -v
            ```
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
    - Linux
        1. Add the NodeSource repository and install node
            ``` bash
            curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
            sudo apt-get install -y nodejs
            ```
            - For distributions that don't use `apt`, refer to the [NodeSource instructions](https://github.com/nodesource/distributions)
        2. Verify the installation
            ``` bash
            node -v
            npm -v
            ```
    - Since node is installed directly, only one version is available at a time. To upgrade, download and run the installer for a newer release. If you find yourself needing to switch node versions between projects, look into a version manager like [nvm](https://github.com/nvm-sh/nvm), the node equivalent of pyenv
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
