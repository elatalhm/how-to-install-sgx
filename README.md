# Why?
Intel has old and new instructions for installing SGX lying around leading to some confusion. Since I recently installed Intel SGX DCAP PCCS, I thought I'd write it up.

# Platform
I'm running this on Ubuntu 22.04.3 LTS, kernel v5.15. It's a development machine with `build-essential` and `python3` already installed. YMMV.

# Versions
The commit date of this file shows how up-to-date these instructions are. I'll try to update whenever possible. I usually refer to instructions/versions elsewhere so this page doesn't become outdated as soon as newer versions of nodejs, sgx, etc. come out.

# Instructions
1. Subscribe to Intel PCS and get an API key using the instructions [here](https://www.intel.com/content/www/us/en/developer/articles/guide/intel-software-guard-extensions-data-center-attestation-primitives-quick-install-guide.html) under "*Subscribe to the Intel PCS*. Stop following the instructions there once you have your API key.
2. Install a specific *supported* nodejs version using the instructions [here](https://github.com/nodesource/distributions/wiki/How-to-select-the-Node.js-version-to-install)
    - Supported nodejs versions at the time of writing are `>= 18.17.0 <= 18.19.1 || >= 20.0.0 <= 20.11.1 || >= 21.0.0 <= 21.5.0`. If you try to install the PCCS package without a supported nodejs version, you'll get an error showing the supported versions in this format. They can also be found [here](https://github.com/intel/SGXDataCenterAttestationPrimitives/blob/main/QuoteGeneration/pccs/README.md#how-to-install)).
    - The reason we need to install nodejs using `apt` is that the `sgx-dcap-pccs` package (which we'll install below) needs it as a requirement, so if we install nodejs some other way (e.g., using the "recommended" methods [here](https://nodejs.org/en/download/package-manager)), it'll pull an old version of nodejs as a dependency. We can probably work around that, but I'd rather keep the installation clean.

3. `sudo apt install cracklib-runtime`
4. If you don't have `python3` and `build-essential`, install them and make sure `python` points to python3.
5. Add the Intel SGX package repo to apt:
    ```
    $ sudo su
    # echo 'deb [arch=amd64] https://download.01.org/intel-sgx/sgx_repo/ubuntu focal main' > /etc/apt/sources.list.d/intel-sgx.list
    # wget -O - https://download.01.org/intel-sgx/sgx_repo/ubuntu/intel-sgx-deb.key | apt-key add -
    # apt update
    # exit
    ```

    - **IMPORTANT**: Since this link might change (and because you shouldn't trust some random instructions on the internet messing with your apt repos), please double check the link to make sure it's up-to-date from [here (an intel.com page)](https://www.intel.com/content/www/us/en/developer/articles/guide/intel-software-guard-extensions-data-center-attestation-primitives-quick-install-guide.html) under "*Set up the Intel PCCS*". Ignore the rest of the instructions on this page (like ones about nodejs versions); just make sure the repo link is correct.

6. `sudo apt install sqlite3` (not entirely sure this is needed but it's mentioned as a prerequisite in the Intel page above.
7. `sudo apt install sgx-dcap-pccs`
    - This will ask you a bunch of questions, including your API key. Good luck.
