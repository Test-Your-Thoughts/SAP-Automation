# Python Installation
- get pyenv from [how to install pyenv](https://github.com/pyenv/pyenv#installation)
- install python 3.12.11 (last supported version for the latest available pyrfc)

  ```
  pyenv install 3.12.11
  ```
  ```
  ${HOME}/.pyenv/versions/3.12.11/bin/python3 --version
  ```
- create a virtual environment

  ```
  mkdir ${HOME}/packages
  ```
  ```
  ${HOME}/.pyenv/versions/3.12.11/bin/python3 -m venv ${HOME}/packages/pyrfc_venv
  ```
  ```
  source ${HOME}/packages/pyrfc_venv/bin/activate
  ```
# SAP Dependencies
- download nwrfcsdk tool kit from [SAP NetWeaver RFC SDK Download](https://me.sap.com/swdcnav/products/_APP=00200682500000001943&_EVENT=DISPHIER&HEADER=Y&FUNCTIONBAR=N&EVENT=TREE&NE=NAVIGATE&ENR=01200314690100002214&V=MAINT)
- unzip the nwrfc750P_*.zip
- copy the nwrfcsdk directory to $HOME/packages directory
- set environment varibles using below commands

  ```
  chmod -R 750 ${HOME}/packages/nwrfcsdk
  export SAPNWRFC_HOME="${HOME}/packages/nwrfcsdk"
  export LD_LIBRARY_PATH="${SAPNWRFC_HOME}/lib"
  ```
# PyRFC Installation
- download pyrfc from [PyRFC Download](https://github.com/SAP-archive/PyRFC/releases/download/v3.3.1/pyrfc-3.3.1-cp312-cp312-linux_x86_64.whl)
- Copy pyrfc to $HOME/packages/pyrfc_venv directory
- install pyrfc using pip

  ```
  pip install --upgrade pip
  pip install "${HOME}/packages/pyrfc_venv/pyrfc-3.3.1-cp312-cp312-linux_x86_64.whl"
  ```
# Test PyRFC Installation
- execute below command to test successful installation of pyrfc (output should return nothing)
  ```
  python3 -c "import pyrfc"
  ```
