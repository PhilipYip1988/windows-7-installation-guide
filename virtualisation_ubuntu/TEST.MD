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
