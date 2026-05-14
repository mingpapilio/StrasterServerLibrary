
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
**Figure 1. Server structure.** StrasterTower acts as the main access and processing point. Users usually use **SSH** to log on to StrasterTower from their personal computers, manage or process data there, and use it to interact with the shared Foster & Stracy storage server. New images from the microscope computer are stored on the storage server, while raw and processed images can also be exchanged with the image analysis computer.

# Initialisation

## Get started

### Configure your account