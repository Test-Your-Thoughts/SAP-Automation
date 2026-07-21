# PyRFC Setup Guide for Linux
# 1. SAP Dependencies.
NOTE: skip this if pyrfc is being installed on sap server
- download nwrfcsdk tool kit from [SAP NetWeaver RFC SDK Download](https://me.sap.com/swdcnav/products/_APP=00200682500000001943&_EVENT=DISPHIER&HEADER=Y&FUNCTIONBAR=N&EVENT=TREE&NE=NAVIGATE&ENR=01200314690100002214&V=MAINT)
- unzip the nwrfc750P_*.zip and place the nwrfcsdk folder in ${HOME}/ directory of your linux server
- set environment varibles using below commands

  ```
  chmod -R 750 ${HOME}/nwrfcsdk
  ```
  ```
  cat << 'EOF' >> ${HOME}/.bashrc

  # PATH variables for SAP NetWeaver RFC SDK -- for PyRFC Compatibilty
  export SAPNWRFC_HOME="${HOME}/nwrfcsdk"
  export LD_LIBRARY_PATH="${SAPNWRFC_HOME}/lib"

  EOF
  
  ```
# 2. Python Installation.
- get pyenv by executing below command (refer to [pyenv documentation](https://github.com/pyenv/pyenv#installation) for more details)
  ```
  curl -fsSL https://pyenv.run | bash
  ```
- execute below command to add pyenv compatabilty to ${HOME}/.bashrc
  ```
  cat << 'EOF' >> ${HOME}/.bashrc

  # Set varaibles -- for pyenv compatability
  export PYENV_ROOT="${HOME}/.pyenv"
  [[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"
  eval "$(pyenv init - bash)"

  EOF

  ```
- source the file for the changes to become effective in the current sheell environment
  ```
  source ${HOME}/.bashrc
  ```
- install python 3.12.11 (last supported version for the latest available pyrfc)

  ```
  pyenv install 3.12.11
  ```
- validate successful installation
  ```
  ${HOME}/.pyenv/versions/3.12.11/bin/python3 --version
  ```
# 3. PyRFC Installation.
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
- download pyrfc
  ```
  cd ${HOME}/packages/pyrfc_venv/
  wget https://github.com/SAP-archive/PyRFC/releases/download/v3.3.1/pyrfc-3.3.1-cp312-cp312-linux_x86_64.whl

  ```
- alternate approach download the wheel file from [PyRFC Download](https://github.com/SAP-archive/PyRFC/releases/download/v3.3.1/pyrfc-3.3.1-cp312-cp312-linux_x86_64.whl) and place it in ${HOME}/packages/pyrfc_venv directory
- install pyrfc using pip

  ```
  pip install --upgrade pip
  ```
  ```
  pip install "${HOME}/packages/pyrfc_venv/pyrfc-3.3.1-cp312-cp312-linux_x86_64.whl"
  ```
# 4. Test PyRFC Installation.
- execute below command to test successful installation of pyrfc (output should return nothing)

  ```
  python3 -c "import pyrfc"
  ```
