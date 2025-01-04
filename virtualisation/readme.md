# Windows 7 Virtualisation

## Download Dell ISO

Download and launch the Windows ISO Download Tool:

* [Windows ISO Download Tool](https://www.heidoc.net/joomla/technology-science/microsoft/67-microsoft-windows-and-office-iso-download-tool)

<img src='./images/img_001.png' alt='img_001' width='600'/>

Select Dell and OptiPlex 7040:

<img src='./images/img_002.png' alt='img_002' width='600'/>

Select Windows 7 64 Bit (or Windows 7 32 Bit) and select Download:

<img src='./images/img_003.png' alt='img_003' width='600'/>

## Windows HyperVisor Platform

Add/Remove Features.

## VMware

Download the latest version of VMware Workstation from VMware:

* [VMware Workstation](https://softwareupdate.vmware.com/cds/vmw-desktop/ws/)




Add the following to your VMware configuration file:

```
firmware = "efi"
acpi.passthru.slic = "TRUE"
acpi.passthru.slicvendor = "TRUE"
SMBIOS.reflecthost = "TRUE"
```

Do not add the first line for Windows 7 32 Bit. For 64 Bit add the first line but check for the line:

```
firmware = "bios"
```

If it exists delete it. This will prevent you from booting to BIOS instead of EFI. Note you will not be able to Boot with EFI if your install.wim on your ISO is greater than 4 GB for instance if you have slipstreamed drivers and updates to the install.wim.