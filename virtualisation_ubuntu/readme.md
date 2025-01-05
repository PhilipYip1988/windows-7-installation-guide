
## Download the Windows ISO Download Tool:

The Windows ISO Download Tool is a Windows Application:

* [Windows ISO Download Tool](https://www.heidoc.net/joomla/technology-science/microsoft/67-microsoft-windows-and-office-iso-download-tool)

## Wine

Open up the Terminal to add the 32 Bit architecture input:

```bash
sudo dpkg --add-architecture i386
```

The following will display Note that `sudo` means super user do and an authentication prompt will display. Input your password to continue:

<img src='./images/img_001.png' alt='img_001' width='600'/>

This will display a new prompt:

<img src='./images/img_002.png' alt='img_002' width='600'/>

To create the directory `keyrings`use:

```bash
sudo mkdir -pm755 /etc/apt/keyrings
```

Note that this is a system wide directory found under `/etc/apt`. Because this is a system wide directory `sudo` is required to run the command make directory `mkdir`. `-p` is an instruction to create the parent directories if required. `-m755` sets permissions `7` (read, write, and execute for the duper user), `5` (read and execute for the group of the user), `5` (read and execute for others).

The wine repository key can be added to this keyring:

```bash
sudo wget "https://dl.winehq.org/wine-builds/winehq.key" -O /etc/apt/keyrings/winehq-archive.key
```

`wget` is a command line tool which is used to download a file from a URL. `-O` is an instruction to output the key to the file `/etc/apt/keyrings/winehq-archive.key`. 

wine has Ubuntu version specific repositories. To check the Ubuntu version use:

```bash
cat /etc/os-release
```

The terminal will display something like: 

<img src='./images/img_003.png' alt='img_003' width='600'/>

In this example the `UBUNTU_CODENAME` in this case is `noble` (Ubuntu 24.04 LTS).

Add only the repository which matches the `UBUNTU_CODENAME`:

<details>
<summary>Ubuntu 24.04 VERSION_CODENAME=noble</summary>

```bash
sudo wget -NP /etc/apt/sources.list.d/ https://dl.winehq.org/wine-builds/ubuntu/dists/noble/winehq-noble.sources
```

</details>


<details>
<summary>Ubuntu 24.10 VERSION_CODENAME=oracular</summary>

```bash
sudo wget -NP /etc/apt/sources.list.d/ https://dl.winehq.org/wine-builds/ubuntu/dists/oracular/winehq-oracular.sources
```

</details>

To update the repositories used by `apt` use:

```bash
sudo apt update
```

To install wine use:

```bash
sudo apt install wine wine32 wine64 libwine fonts-wine winetricks playonlinux
```

The package `wine` allows Windows applications to run on Ubuntu. `wine32` allows Windows 32 Bit applications to run on 64 Bit Ubuntu. `wine64` allows Windows 64 Bit applications to run on 64 Bit Ubuntu.

The package `libwine` is a core dependency of `wine`. The package `fonts-wine` is required for proper font rendering.

`wine-mono` is a .NET Framework replacement for wine:

```bash
wget "https://dl.winehq.org/wine/wine-mono/9.4.0/wine-mono-9.4.0-x86.msi"
```

```bash
wine start wine-mono-9.4.0-x86.msi
```

`wine-gecko` is a replacement for Internet Explorer for wine:

```bash
wget "https://dl.winehq.org/wine/wine-gecko/2.47.4/wine-gecko-2.47.4-x86_64.msi"
```

```bash
wine start wine-gecko-2.47.4-x86_64.msi
```

## Windows ISO Download Tool

To download the Windows ISO Tool use:


```bash
wget "https://www.heidoc.net/php/Windows-ISO-Downloader.exe"
```

To launch it using wine use:

```bash
wine start Windows-ISO-Downloader.exe
```

## Installing Build Essential

Install the `build-essential` package:

```bash
sudo apt install build-essential
```

## Downloading VMware Workstation

Download the compatible version of VMware Workstation from VMware. This is in the core folder and has the extensions `.bundle.tar`:

* [VMware Workstation](https://softwareupdate.vmware.com/cds/vmw-desktop/ws/)

Note that it takes time for the host modules to be developed and therefore there may not be a host module for the latest version of VMware Workstation Player.

## Installing VMWare Workstation

Extract the `.tar` to get to the extracted folder with the `.bundle` file. Right click the `.bundle` file and select properties:


Select Executable as Program:


Right click empty space in the folder and select open in terminal.

In the terminal input the following command with a space (but don't run this command):

```bash
chmod +x 
```

This command changes the permissions of a file to execution. Drag the bundle file into the terminal and run the command:

```bash
chmod +x '/home/philip/Downloads/VMware-Workstation-17.5.1-23298084.x86_64.bundle/VMware-Workstation-17.5.1-23298084.x86_64.bundle' 
```

Now that the file is executable it can be run as a super using. Input the following command with a space (but don't run this command):

```bash
sudo 
```

Drag the bundle file into the terminal and run the command:

```bash
sudo '/home/philip/Downloads/VMware-Workstation-17.5.1-23298084.x86_64.bundle/VMware-Workstation-17.5.1-23298084.x86_64.bundle' 
```

## Disable Secure Boot






## SLIC Passthrough and OEM SLP

To check if the host PC has a SLIC input:

```bash
cd /sys/firmware/acpi/tables/
```

Then:

```bash
dir
```

If a SLIC is present it can be passed through to the host PC. Add the following to your VMware configuration file:

```
acpi.passthru.slic = "TRUE"
acpi.passthru.slicvendor = "TRUE"
SMBIOS.reflecthost = "TRUE"
```


























## Install VMware Host Modules 


The following GitHub repository contains modded VMware Host Modules which are required for the Virtual Monitor Kernel Module and Virtual Network Adaptor Module to load properly. Download the latest version:

* [GitHub: VMware host modules](https://github.com/mkubecek/vmware-host-modules)

To the left hand side select releases:



Download the latest release as a `.zip` file:



VMWare is installed however the Virtual Monitor Kernel Module and Virtual Network Kernel Module are not loaded correctly. When the VM is launched the following error message displays:

>>> Could not open /dev/vmmon: No such file or directory.
>>> Please make sure that the kernel module `vmmon' is loaded.


Open the extracted folder `vmware-host-modules` in the terminal. Input:

```bash
tar -cf vmmon.tar vmmon-only
```

```bash
tar -cf vmnet.tar vmnet-only
```

Copy these to the location `/usr/lib/vmware/modules/source`:

```bash
sudo cp -v vmmon.tar vmnet.tar /usr/lib/vmware/modules/source
```

Install the modules:

```bash
sudo vmware-modconfig --console --install-all
```
























## MOK Management (Not Working)

In order for these kernel modules to be loaded at startup, a Machine Owner Key (MOK) is required.
Generate a new Machine Owner Key:

```bash
openssl req -new -x509 -newkey rsa:2048 -keyout VMWARE1751.priv -outform DER -out VMWARE1751.der -nodes -days 36500 -subj "/CN=VMWARE/"
```

Sign the kernel module vmmon:

```bash
sudo /usr/src/linux-headers-$(uname -r)/scripts/sign-file sha256 ./VMWARE1751.priv ./VMWARE1751.der $(modinfo -n vmmon)
```

Sign the kernel module vmnet:

```bash
sudo /usr/src/linux-headers-$(uname -r)/scripts/sign-file sha256 ./VMWARE1751.priv ./VMWARE1751.der $(modinfo -n vmnet)
```

Importing the MOK with MOK management system:

```bash
sudo mokutil --import VMWARE1751.der
```

In the terminal create a MOK password for example:

```
vmware1751
```

Confirm the password:

```
vmware1751
```

Input:

```bash
sudo reboot
```












Uninstall

```bash
cd /usr/bin
```

```bash
sudo vmware-installer -u vmware-workstation
```