# Installing VSCode/getting your data science development environment setup
## Downloads
* [Visual Studio Build Tools](https://visualstudio.microsoft.com/thank-you-downloading-visual-studio/?sku=BuildTools&rel=16)
    * C++ compiler for package building
* [Miniconda](https://docs.conda.io/en/latest/miniconda.html) or [Miniforge](https://github.com/conda-forge/miniforge)
    * our main way of interacting with python, a minimal version of anaconda, if we don't have enterprise licensing with Anaconda, we really should use miniforge instead
* [VSCodium](https://vscodium.com/) OR [VSCode](https://code.visualstudio.com/)
    * our IDE. I prefer VSCodium as it [removes the telemetry and has binaries licensed under the  MIT license](https://vscodium.com/#why) microsoft adds. You might prefer VSCode as it has github copilot chat.

## Steps
1. Download all the files from the downloads section
2. Install Visual Studio Build Tools (windows)
    1. Once it finishes, run it, during the setup you'll reach a tab called `Workloads`
    2. within `Workloads`, select C++ Buildtools
    3. Run through the installation
3. Install Miniforge
    1. Agree to the licensing terms
    2. within the install options, Select `Just Me (recommended)`, then click next
    3. Install to the default directory, then click next
    4. within the advanced options, make sure `Register Miniforge as the System Python3.X`
    5. Complete the installation
4. Install VSCodium
    1. Accept the licensing terms
    2. within the `Select Additional Tasks` tab, make sure the following is selected:
        * `Add "Open with VSCodium" action to Windows Explorer file context menu`
        * `Add "Open with VSCodium" action to Windows Explorer directory context menu`
        * `Add to PATH (requires shell restart)`
    3. Continue to the next step
5. Launch VSCodium
    1. Navigate to the extensions tab (Ctrl + Shift + X)
    2. Within the search tab search for python, select it and install it
    3. After the python extension is installed, navigate to your VSCodium Settings (Ctrl + ,)
    4. Find extensions section of settings, select it, and find python, select it
    5. Within the python section, look for `Conda Path`, set it to be the path to your installation of miniconda. For me it was `C:\Users\$USERNAME\Miniconda3` replace `$USERNAME` with the username with your username
    6. Import the [vs-dev](/development-environments/vs-dev.code-profile) code profile. This will load the same extensions I use
6. (optional) Import system certificates (windows)
   - the following instructions are to resolve errors when installing pip and conda packages such as
     - ```
        [SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed: unable to get local issuer certificate (_ssl.c:997)')))
        ```
     - ```
        CondaSSLError: Encountered an SSL error. Most likely a certificate verification issue.

        Exception: HTTPSConnectionPool(host='conda.anaconda.org', port=443): Max retries exceeded with url: /conda-forge/win-64/current_repodata.json (Caused by SSLError(SSLCertVerificationError(1, '[SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed: unable to get local issuer certificate (_ssl.c:997)')))
        ```
    - we will be adding the zscaler certificate to python's certificate store and setting it as the environment variable
   1. Download zscaler certificate from IT.
      - look for `Cert.pem` file. 
    2. Open the certificate in notepad or a text editor
    3. Copy the contents of the file
    4. Navigate to your certifi folder, for me it was `C:\Users\$USERNAME\AppData\Local\miniforge3\Lib\site-packages\certifi`
       1. Replace `$USERNAME` with your username. You can also copy the path and paste it into the windows file explorer
       2. Within the folder, open the file called `cacert.pem`
          - it should look something like this
          - ``` pem
            ----END CERTIFICATE-----
            ----BEGIN FANTASY CERTIFICATE-----
            # Issuer: CN=Mythical Security Fantasy Root 2023 O=Imaginary Security Firm
            # Subject: CN=Mythical Security Fantasy Root 2023 O=Imaginary Security Firm
            # Label: "Mythical Security Fantasy Root 2023"
            # Serial: 123456789012345678901234567890123456789
            # MD5 Fingerprint: aa:bb:cc:dd:ee:ff:11:22:33:44:55:66:77:88:99:00
            # SHA1 Fingerprint: 11:22:33:44:55:66:77:88:99:00:aa:bb:cc:dd:ee:ff:11:22:33:44
            # SHA256 Fingerprint: 00:11:22:33:44:55:66:77:88:99:00:aa:bb:cc:dd:ee:ff:11:22:33:44:55:66:77:88:99:00
            ----END FANTASY CERTIFICATE-----

            ``` 
       3. Paste the contents of the certificate at the end of the file after the last line of the file
          - the file should look something like this
          - ```
                        ----BEGIN FANTASY CERTIFICATE-----
            # Issuer: CN=Mythical Security Fantasy Root 2023 O=Imaginary Security Firm
            # Subject: CN=Mythical Security Fantasy Root 2023 O=Imaginary Security Firm
            # Label: "Mythical Security Fantasy Root 2023"
            # Serial: 123456789012345678901234567890123456789
            # MD5 Fingerprint: aa:bb:cc:dd:ee:ff:11:22:33:44:55:66:77:88:99:00
            # SHA1 Fingerprint: 11:22:33:44:55:66:77:88:99:00:aa:bb:cc:dd:ee:ff:11:22:33:44
            # SHA256 Fingerprint: 00:11:22:33:44:55:66:77:88:99:00:aa:bb:cc:dd:ee:ff:11:22:33:44:55:66:77:88:99:00
            ----END FANTASY CERTIFICATE-----
                -----BEGIN CERTIFICATE-----
                ZSCALERCERTIFICATEHERE
                -----END CERTIFICATE-----
            ```
    5. Add powershell environment variable
       1. Open powershell as an administrator
       2. Run the following command
          ``` powershell
          [System.Environment]::SetEnvironmentVariable("REQUESTS_CA_BUNDLE", "C:\Users\$USERNAME\AppData\Local\miniforge3\Lib\site-packages\certifi\cacert.pem", "Machine")
          ```
       3. Try installing a package with pip or conda, if it works, you're done
          - ```
            pip install -U certifi
            ```
          - ```
            conda install -c conda-forge certifi
            ```
       4. If neither of those work and you still get a certificate error, run the following command, this will temporarily set the environment variable for the current powershell session. 
          ``` powershell
          $env:REQUESTS_CA_BUNDLE="C:\Users\$USERNAME\AppData\Local\miniforge3\Lib\site-packages\certifi\cacert.pem"
          ```
        5. If you are still having issues, try updating your conda and pip configuration to point to that certificate file
           - ``` 
             conda config --set ssl_verify C:\Users\$USERNAME\AppData\Local\miniforge3\Lib\site-packages\certifi\cacert.pem```
           - ```
             pip config set global.cert $(python -m certifi) 
        6. Validate the commands worked by running the following commands
           - ```
               conda config --show ssl_verify
               ```
             - you should see the path to the certificate
             - ```
                 ssl_verify: C:\Users\$USERNAME\AppData\Local\miniforge3\lib\site-packages\certifi\cacert.pem
                 ```
            - ```
                pip config list```
            - you should see the path to the certificate
                - ```
                   global.cert='C:\Users\$USERNAME\AppData\Local\miniforge3\lib\site-packages\certifi\cacert.pem' 
                   ```


## Additional debugging
* requests is throwing certificate errors
    ```
    requests.exceptions.SSLError: HTTPSConnectionPool(host='www.google.com', port=443): Max retries exceeded with url: / (Caused by SSLError(SSLCertVerificationError(1, '[SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed: unable to get local issuer certificate (_ssl.c:997)')))
    ```
    * the solution is to define a environment file to reference the location of your certificate path:
        > Requests will automatically search for any valid certificate in the `REQUESTS_CA_BUNDLE` Env variable.
        > To configure the REQUESTS_CA_BUNDLE environment variable:
        > On Windows, run the following PowerShell command as Administrator:
        ```    
            [System.Environment]::SetEnvironmentVariable("REQUESTS_CA_BUNDLE", "<Path to Certificate>\ca-bundle.pem", "Machine")
        ```
    * replace `<Path to Certificate>` with the certificate path
    * running the requests ca command didn't work alternate solution
      * ``` powershell
        $env:REQUESTS_CA_BUNDLE="C:\Users\$USERNAME\AppData\Local\miniforge3\Lib\site-packages\certifi\cacert.pem"
        ```
* Help conda is not activating in powershell!
    * simply run `conda init` in powershell and restart your terminal window