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

## Installing VMware Workstation

Download the latest version of VMware Workstation from VMware:

* [VMware Workstation](https://softwareupdate.vmware.com/cds/vmw-desktop/ws/)

## Creating a Windows 7 VM


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
