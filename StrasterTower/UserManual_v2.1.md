
# 1. About the Straster Tower
Our server was purchased under the ERC and Wellcome Trust fungings. Many thanks to Kevin, Mathew, and Laura for providing such a great computational resource! This document is intended to be a walk-through tutorial and toolbox. 

Some housekeeping notes to mention first:

- Always use __htop__ or top to check current traffic before launching heavy tasks.
- __Read the welcome message__ to check disk storage and scheduled maintenance session.
- This machine is a __computation device__, not for long-term storage. Please always assume data is at risk and save important files to your local device or an appropriate backup location.
- Feel free to work in your personal Python environment, but __do not__ install R packages system-wide yourself.

## 1.1 Drive structure and intended use
The server contains four drives:

- Two solid state drives (SSD)
  - One system drive (500 GB)
  - One 4 TB drive located at `/drives/4tb`
- Two hard disk drives (HDD)
  - One 12 TB drive located at `/drives/12tb_foster`
  - One 12 TB drive located at `/drives/12tb_stracy`

In general:

- Use __`/drives/4tb`__ for simulations, numerical work, and statistical analysis.
- Use __`/drives/12tb_foster`__ and __`/drives/12tb_stracy`__ mainly for metagenomic work.
- Your user account will be assigned to either the Foster or Stracy group during setup.

The folder layout is organised as follows:

- Personal work folder on the 4 TB drive: `/drives/4tb/<your_username>`
- Shared folder on the 4 TB drive: `/drives/4tb/shared`
- Personal work folder on the relevant 12 TB drive:
  - `/drives/12tb_foster/<your_username>` or
  - `/drives/12tb_stracy/<your_username>`
- Shared group folder on the relevant 12 TB drive:
  - `/drives/12tb_foster/shared` or
  - `/drives/12tb_stracy/shared`

## 1.2 The relationships between machines
![Server structure diagram](images/Server%20structure.png)
**Server structure.** StrasterTower acts as the main access and processing point. Users usually use **SSH** to log on to StrasterTower from their personal computers, manage or process data there, and use it to interact with the shared Foster & Stracy storage server. New images from the microscope computer are stored on the storage server, while raw and processed images can also be exchanged with the image analysis computer.

# 2. Initialisation
## 2.1 Get started
### 2.1.1 Configure your account
Please find Ming or Laura to set up an account.

During setup, we will:

- create your user account,
- assign you to the appropriate group (`foster` or `stracy`),
- create your personal folders on the relevant drives.

### 2.1.2 Connect to the server
Once you __activate the Oxford VPN__, you can access the server via:
```bash
ssh $USER@strastertower.path.ox.ac.uk
```
Replace `$USER` with your username. After typing the password, you should be in the server.

### 2.1.3 Move to your personal folder
After logging in, you should usually move to your personal folder on the 4 TB drive:
```bash
cd /drives/4tb/$USER
```
__PLEASE ALWAYS REMEMBER__ to move to your intended working directory whenever you log on to the server.

# 3. Install essential applications
## 3.1 Install Miniconda/Miniforge
We recommend installing Miniconda inside your personal folder on the 4 TB drive. You will manage your own environment so there will be

```bash
# Go to your personal folder on the 4 TB drive
cd /drives/4tb/$USER

# Download Miniconda installer
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O miniconda.sh

# Install Miniconda into your personal folder
bash miniconda.sh -b -p /drives/4tb/$USER/miniconda3

# Remove installer
rm miniconda.sh

# Activate Conda for this shell
source /drives/4tb/$USER/miniconda3/bin/activate

# Set up Conda for future bash sessions
conda init bash

# Reload shell configuration
source ~/.bashrc

# Check installation
conda --version
```

__Important__: do __not__ manually create the `miniconda3` folder before running the installer.

See [here](https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html) for more information about managing conda environments.

## 3.2 Install JupyterLab
Assuming you are already inside the correct conda environment, you can install JupyterLab with:
```bash
conda install -c conda-forge jupyterlab -y
```

You can confirm that it was installed correctly with:
```bash
jupyter lab --version
```
# 4. Useful commands and tools

## 4.1 Testing JupyterLab
The following commands show how to activate JupyterLab, assuming you start from a fresh terminal tab.

```bash
# Connect to the server
ssh $USER@strastertower.path.ox.ac.uk

# Move to your intended working directory before launching JupyterLab
cd /drives/4tb/$USER

# Launch JupyterLab on your preferred port
# Please choose a favourite port between 8800 and 8900, and keep using it consistently
jupyter lab --port=8888
```

If activation is successful, you should see URLs printed in the terminal. We will use the generated token to connect through the browser.

<img src="images/connection.png" alt="Successful SSH login to StrasterTower" width="60%">

**Successful SSH login to StrasterTower.** After connecting with `ssh username@strastertower.path.ox.ac.uk`, users should see the Ubuntu login message, current disk-usage summary, and the StrasterTower welcome/reminder box. This confirms that the SSH connection has reached StrasterTower successfully.

<img src="images/activate_jupyter.png" alt="Successful launching jupyter" width="100%">

**Jupyter server startup message.** After starting Jupyter, the terminal should show that Jupyter Server is running and provide two browser URLs containing an access token. Both URLs shown near the bottom can be copied and pasted. To activate the link from your own computer, keep this Jupyter terminal session open and use another terminal tab to set up the connection; this will be explained in the next section.

__Open another terminal tab__ to create the connection to JupyterLab through the chosen port.

```bash
# Replace 8888 with your preferred port between 8800 and 8900
ssh -N -f -L 8888:localhost:8888 $USER@strastertower.path.ox.ac.uk
```

Now paste the URL shown by JupyterLab into your browser. It should look something like:

`http://localhost:8888/lab?token=...`

You can then:

1. run or edit files through the tabs inside JupyterLab,
2. drag files to the file browser on the left side to upload them,
3. download files by right-clicking them in the file browser.

`Folders` would need to be `zipped` before download.

<img src="images/jupyter.png" alt="Successful jupyter browser" width="80%">

**JupyterLab opened successfully.** A successful connection opens JupyterLab in the local web browser at `localhost:8890/lab`. The file browser on the left shows the server directory, while the terminal or notebook panel on the right runs on StrasterTower, not on the user’s personal computer.

## 4.2 Common linux commands
