# Installation

## Overview

The following section provides instructions on how to install the GCloud CLI on Windows, MacOS, and Linux.

### I**nstalling gcloud CLI on Windows**

**Step 1: Download the Installer**

* Go to the Google Cloud SDK installer page for Windows.
* Download the **Cloud SDK Installer** (an `.exe` file).

**Step 2: Run the Installer**

* Once downloaded, run the installer.
* During installation, select the option to install **gcloud CLI**, **kubectl**, and any other optional components you may need.
* Make sure to allow the installer to run `gcloud init` after installation, which will help set up your configuration.

**Step 3: Complete the Installation**

* Follow the on-screen instructions to finish the installation.
* When prompted, allow the installer to install Python (if it is not already installed).

**Step 4: Initialize gcloud**

*   Open a **Command Prompt** (or **PowerShell**) and run the following command to initialize gcloud:

    ```bash
    gcloud init
    ```
* This will guide you through setting up the default project, configuration, and authentication.

**Step 5: Verify Installation**

*   To verify that gcloud has been installed successfully, run the following command:

    ```bash
    gcloud version
    ```

### **Installing gcloud CLI on Linux**

**Step 1: Update Your Package Index**

*   Open a terminal and update the package list:

    ```bash
    sudo apt-get update
    ```

**Step 2: Install Prerequisites (Debian/Ubuntu-based)**

*   Install the following packages:

    ```bash
    sudo apt-get install apt-transport-https ca-certificates gnupg
    ```

**Step 3: Add the Google Cloud SDK Repository**

*   Add the Google Cloud package source:

    ```bash
    echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] https://packages.cloud.google.com/apt cloud-sdk main" | sudo tee -a /etc/apt/sources.list.d/google-cloud-sdk.list
    ```
*   Import the Google Cloud public key:

    ```bash
    curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo tee /usr/share/keyrings/cloud.google.gpg
    ```

**Step 4: Install the Google Cloud SDK**

*   Install the gcloud CLI:

    ```bash
    sudo apt-get update && sudo apt-get install google-cloud-sdk
    ```

**Step 5: Initialize gcloud**

*   After installation, initialize the gcloud CLI:

    ```bash
    gcloud init
    ```

**Step 6: Verify Installation**

*   Check if gcloud is installed and the version is correct:

    ```bash
    bashCopy codegcloud version
    ```

**Alternate Installation Method (Using Script)**

*   You can also install gcloud using the interactive installer script:

    ```bash
    curl -O https://dl.google.com/dl/cloudsdk/channels/rapid/downloads/google-cloud-sdk-<VERSION>-linux-x86_64.tar.gz
    ```
* Replace `<VERSION>` with the latest version number.

### **Installing gcloud CLI on macOS**

**Step 1: Install Using Homebrew (Recommended)**

*   If you have **Homebrew** installed, run the following command in your terminal to install the gcloud CLI:

    ```bash
    brew install --cask google-cloud-sdk
    ```

**Step 2: Add gcloud CLI to Your Path**

*   After installation, you will need to ensure that the `gcloud` command is added to your terminal path. Run:

    ```bash
    echo 'source "/usr/local/Caskroom/google-cloud-sdk/latest/google-cloud-sdk/path.bash.inc"' >> ~/.bash_profile
    source ~/.bash_profile
    ```

    (If you're using **zsh**, modify the `.zshrc` file instead of `.bash_profile`).

**Step 3: Initialize gcloud**

*   Initialize the gcloud CLI by running:

    ```bash
    bashCopy codegcloud init
    ```
* This will prompt you to authenticate your account and set up your initial configuration.

**Step 4: Verify Installation**

*   Check that the installation was successful:

    ```bash
    gcloud version
    ```

**Alternate Installation Method (Download and Install Manually)**

* You can also download the Google Cloud SDK directly from the Google Cloud SDK page.
*   Extract the downloaded archive and run the install script:

    ```bash
    ./google-cloud-sdk/install.sh
    ```

***

### **Common Post-Installation Steps**

1. **Authenticate with Google Cloud**:
   * After running `gcloud init`, you’ll be prompted to authenticate your account. Open the URL in a web browser and sign in with your Google Cloud credentials.
2. **Set Default Project and Region**:
   * During the initialization, you can set your default Google Cloud project and region, which will be used for all commands unless specified otherwise.
3. **Update gcloud CLI**:
   *   You can update your gcloud installation by running:

       ```bash
       gcloud components update
       ```
4. **Additional Components**:
   *   If you need **alpha** or **beta** components, you can install them with:

       ```bash
       gcloud components install alpha
       gcloud components install beta
       ```
