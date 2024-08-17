# Multiple SSH Keys for separate accounts
This guide was heavily inspired by the following gists
- https://gist.github.com/alejandro-martin/aabe88cf15871121e076f66b65306610
  - for the general ssh config setup
- https://gist.github.com/yinzara/bbedc35798df0495a4fdd27857bca2c1
  - for the gitconfig routing

## Creating SSH Keys
Follow the guides from the github documenation page [Generating a new SSH key and adding it to the ssh-agent](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)

1. In a terminal, run the command 
    ```ssh-keygen -t ed25519 -C "your_email@example.com"`
    **Note:** If you are using a legacy system that doesn't support the Ed25519 algorithm, use:

    ```
    ssh-keygen -t ed25519 -b 4096 -C "your_email@example.com"
    ```

    This creates a new SSH key, using the provided email as a label.

    ```
    > Generating public/private ALGORITHM key pair.
    ```

    When you're prompted to "Enter a file in which to save the key", you can press **Enter** to accept the default file location. Please note that if you created SSH keys previously, ssh-keygen may ask you to rewrite another key, in which case we recommend creating a custom-named SSH key. To do so, type the default file location and replace id\_ALGORITHM with your custom key name.

    ```
    > Enter a file in which to save the key (/Users/YOU/.ssh/id_ALGORITHM): [Press enter]
    ```
2. Activate the ssh agent. These commands will change for your platform
    - linux/mac (bash)
        ```
        $ eval "$(ssh-agent -s)"
        > Agent pid 59566
        ```
      - for those who use fish shell, remove the `$`
    - windows
        ```
        # start the ssh-agent in the background
        Get-Service -Name ssh-agent | Set-Service -StartupType Manual
        Start-Service ssh-agent
        ```
3. Add your SSH private Key to ssh-agent replace `id_ed25519` with the name of the file stored in the ssh folder
    - linux
        ```
        ssh-add ~/.ssh/id_ed25519
        ```
    - mac
        If you're using macOS Sierra 10.12.2 or later, you will need to modify your `~/.ssh/config` file to automatically load keys into the ssh-agent and store passphrases in your keychain.

    -   First, check to see if your `~/.ssh/config` file exists in the default location.

        ```
        $ open ~/.ssh/config
        > The file /Users/YOU/.ssh/config does not exist.
        ```

    -   If the file doesn't exist, create the file.

        ```
        touch ~/.ssh/config
        ```

    -   Open your `~/.ssh/config` file, then modify the file to contain the following lines. If your SSH key file has a different name or path than the example $secondary-repos-folder, modify the filename or path to match your current setup.

        Text
        ```
        Host github.com
        AddKeysToAgent yes
        UseKeychain yes
        IdentityFile ~/.ssh/id_ed25519

        ```

    **Notes:**

    -   If you chose not to add a passphrase to your key, you should omit the `UseKeychain` line.

    -   If you see a `Bad configuration option: usekeychain` error, add an additional line to the configuration's' `Host *.github.com` section.

        Text

        -   ```
                Host github.com
                IgnoreUnknown UseKeychain
            ```

    -   Add your SSH private key to the ssh-agent and store your passphrase in the keychain. If you created your key with a different name, or if you are adding an existing key that has a different name, replace *id\_ed25519* in the command with the name of your private key file.

        ```
        ssh-add --apple-use-keychain ~/.ssh/id_ed25519

        ```

        **Note:** The `--apple-use-keychain` option stores the passphrase in your keychain for you when you add an SSH key to the ssh-agent. If you chose not to add a passphrase to your key, run the command without the `--apple-use-keychain` option.

        The `--apple-use-keychain` option is in Apple's standard version of `ssh-add`. In macOS versions prior to Monterey (12.0), the `--apple-use-keychain` and `--apple-load-keychain` flags used the syntax `-K` and `-A`, respectively.

        If you don't have Apple's standard version of `ssh-add` installed, you may receive an error. For more information, see "[Error: ssh-add: illegal option -- apple-use-keychain](https://docs.github.com/en/authentication/troubleshooting-ssh/error-ssh-add-illegal-option----apple-use-keychain)."

        If you continue to be prompted for your passphrase, you may need to add the command to your `~/.zshrc` file (or your `~/.bashrc` file for bash).

    -   Add the SSH public key to your account on GitHub. For more information, see "[Adding a new SSH key to your GitHub account](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)."
    - windows
      -    In a terminal window without elevated permissions, add your SSH private key to the ssh-agent. If you created your key with a different name, or if you are adding an existing key that has a different name, replace *id\_ed25519* in the command with the name of your private key file.
        - ```
            ssh-add c:/Users/YOU/.ssh/id_ed25519
          ```
4. With the ssh keys created, add it to your github account. follow the instructions from the github documentation page: [Adding a new SSH key to your GitHub account](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)
    - note: for enterprise sso accounts, you'll need to authorize the ssh key with sso see the instructions [Authorizing an SSH key for use with SAML single sign-on](https://docs.github.com/en/enterprise-cloud@latest/authentication/authenticating-with-saml-single-sign-on/authorizing-an-ssh-key-for-use-with-saml-single-sign-on)

## adding multiple ssh keys
If you work in an environment that uses multiple ssh keys and logins, the following steps will help configure your git in your terminal to change between 2 ssh keys (a `primary` and `secondary` key) for each respective `primary` and `secondary` folder. Feel free to modify these steps if you have more than 2 keys
1. Create or modify your ssh config file in my case it is found within `~/.ssh/config` using a preferred text editor
2. Add the following configuration
   - note: you should have the following variables within your file. Different OS's will have additional parameters needed. 
     - `Host`: this is a title for your reference, come up with a name that will help you differentiate between the entries
     - `HostName`: **IMPORTANT** this is where your repo is stored, for this guide, we are using `github.com`, but can also be `gitlab.com` or your org's respective git repos
     - `IdentityFile`: the path to your ssh private key
    ```
    Host github.com-primary
      HostName github.com
      AddKeysToAgent yes
      UseKeychain yes
      IdentityFile ~/.ssh/id_ed25519_primary

    Host github.com-secondary
      HostName github.com
      AddKeysToAgent yes
      UseKeychain yes
      IdentityFile ~/.ssh/id_ed25519_secondary
    
    ```
3. To automatically switch between the two keys, we will configure the global git to route to separate configs for a given directory. The instructions have been adapted from [@yinzara github_bitbucket_multiple_ssh_keys](https://gist.github.com/yinzara/bbedc35798df0495a4fdd27857bca2c1)
   - note: if we stop at the previous step, you can activate the ssh keys manually using `ssh-add $key_path` where `$key_path` is the path of the given key. 
   1.  Open your git config file using the following command `git config --global --edit`
   2.  Add the following lines replace `/$primary-repos-folder/` and `/$secondary-repos-folder/` with the folders you want each respective key active in. 
    ```
    [includeIf "gitdir:/$primary-repos-folder/"]
       path = ~/$primary-repos-folder/.gitconfig

   [includeIf "gitdir:/$secondary-repos-folder/"]
       path = ~/$secondary-repos-folder/.gitconfig
    ```
   3.  Save your changes and exit from the file
4.  Create a `.gitconfig` for your `primary` folder
    1.  Navigate to your `primary` folder and create a `.gitconfig` file with your preferred text editor
    2.  Within the file, add the following lines
        ```
        [user]
        email = your.email@example.com

        [url "git@github.com:$primary-account-name/"] 
        insteadOf = git@github.com/
        ```
        - note: the `$primary-account-name` will change. For personal repos, it will just be the user's github account name. For organizations, it will be the org name within the `Organizations` tab of github
5.  Repeat the previous step, but replace the `primary` steps with `secondary` for the folders and accounts
6.  Write a shell function to automatically change your ssh keys when you initiate a `git` + `clone`, `fetch`, `pull`, or `push`. I use [fish shell](https://fishshell.com/), so adapt this function for your respective shell
    1.  Navigate and open to the fish config file with your preferred text editor, for m, it is stored in `~/.config/fish/config.fish`
    2.  Paste the following function into the file. Replace the variables: `primary-repos-folder`, `secondary-repos-folder`, `id_ed25519_primary`, `id_ed25519_secondary` with the paths of your respective folders and ssh file paths
        - at a high level this function is performing the following sequence
          1. Set the path variables
          2. Listen for when the command `git` + `clone`, `fetch`, `pull`, or `push`that requires ssh is called
          3. Set a temporary ssh key file
          4. Check which folder to use the ssh key based on the current directory
          5. Check which ssh key is currently active, if the key does not match with the directory, remove all keys using `ssh-add -D`, then activate the correct key `ssh-add $key-path`
          6. Run the original git command
        ```
            # function for switching between ssh logins
            function git
                # Set the real paths for primary and secondary folders and SSH keys as variables
                    set -l primary_path "$HOME/primary-repos-folder"
                    set -l secondary_path "$HOME/secondary-repos-folder"
                    set -l primary_key_path "$HOME/.ssh/id_ed25519_primary"
                    set -l secondary_key_path "$HOME/.ssh/id_ed25519_secondary"

                # Get only the first argument to check the type of git command
                set -l git_command $argv[1]

                # Check if the command involves remote operations that might require SSH
                if string match -r 'push|pull|fetch|clone' -- $git_command
                    set -l ssh_key_file "/tmp/active_ssh_key"
                    set -l primary (realpath $primary_path)
                    set -l secondary (realpath $secondary_path)

                    # Determine which SSH key to use based on the current directory
                    if string match -q -- "$primary/*" "$PWD"
                        set new_key (realpath $primary_key_path)
                        echo "Using primary key: $new_key"
                    else if string match -q -- "$secondary/*" "$PWD"
                        set new_key (realpath $secondary_key_path)
                        echo "Using secondary-repos-folder key: $new_key"
                    else
                        echo "No specific SSH key configured for directory: $PWD"
                        command git $argv
                        return
                    end

                    # Check and manage SSH key
                    if test -f $ssh_key_file
                        set -l active_key (cat $ssh_key_file)
                        echo "Active key: $active_key, New key: $new_key"
                        if test "$active_key" = "$new_key"
                            echo "SSH key $new_key is already active"
                        else
                            echo "Switching to new SSH key: $new_key"
                            ssh-add -D && ssh-add $new_key
                            echo $status " - SSH add status"
                            echo $new_key > $ssh_key_file
                        end
                    else
                        echo "Activating SSH key for the first time: $new_key"
                        ssh-add -D && ssh-add $new_key
                        echo $status " - SSH add status"
                        echo $new_key > $ssh_key_file
                    end
                end

                # Run the git command regardless of whether we changed the SSH key
                command git $argv
            end

        ```
    3.  Reload all active terminals using the following command `source ~/.config/fish/config.fish`
    4.  Navigate to a repo within the primary directory you initialized and run a `git` + `clone`, `fetch`, `pull`, or `push`
    5.  Confirm that the correct ssh key is being used. If not use the following commands to debug further:
        -   list all commands active keys using `ssh-add -l`
        -   debug git ssh command outputs using `GIT_SSH_COMMAND="ssh -vvv" git pull`
        -   test the ssh connections `ssh -T git@github.com`. Note this will be with the key that was previously activated, Run another `git` + `clone`, `fetch`, `pull`, or `push` to reset it
        -   if you encounter any issues with the VS Codium/Code terminals, make sure to kill the terminal and start a new one
7.  You should be all set!

