copied from https://gist.github.com/sgherbst/aee090ce4b9d3df0ea34c81c8f269238

## Prerequisites
1. If you haven't already, [create a free Xilinx account](https://www.xilinx.com/registration/create-account.html).

## Installing Vivado HLx 2020.1 
1. On your local machine, download the [Xilinx Unified Installer 2020.1 Linux Self Extracting Web Installer](https://www.xilinx.com/member/forms/download/xef.html?filename=Xilinx_Unified_2020.1_0602_1208_Lin64.bin) (you'll have to login with your Xilinx account to do this).
2. Make the Vivado installer executable and run it:
```shell
> chmod +x Xilinx_Unified_2020.1*.bin
> sudo ./Xilinx_Unified_2020.1*.bin
```
5. Once the installer loads, click ``Next``.
6. Then enter your Xilinx username and password and leave ``Download and Install Now`` selected.  Click ``Next``.
7. Click ``I Agree`` to all three statements if you agree :-).  Click ``Next``.
8. Select ``Vitis`` and click ``Next``.
9. Under ``Design Tools``:
    1. In ``Vitis Unified Software Platform``, make sure that only ``Vitis`` and ``Vivado`` are selected.
    2. Keep ``DocNav`` checked.
10. Under ``Devices`` menu, de-select all parts except ``Devices for Custom Platforms`` → ``SoCs`` → ``Zynq UltraScale+ MPSoC``.  This should cut down the install size by 30-40 GB.
11. The download size should now be about 18.26 GB and the disk space required will be about 86.37 GB.  It's too bad that these tools take up so much space, but that's unfortunately how it is... :-(
12. Click ``Next`` to proceed to ``Select Destination Directory``.
13. Under ``Installation Options`` → ``Select the installation directory``, use ``/tools/Xilinx`` (create that directory first if necessary).
14. Under ``Select shortcut and file association options``, I usually uncheck ``Create program group entries`` and ``Create desktop shortcuts``.  But you can keep those if you want.
15. Click ``Install`` and wait for the installer to finish.  It will likely take 1-3 hours.
16. Install the Xilinx cable drivers:
```shell
> cd /tools/Xilinx/Vivado/2020.1/data/xicom/cable_drivers/lin64/install_script/install_drivers
> sudo ./install_drivers
```
13. Do some permissions cleanup:
```shell
> sudo chown -R $USER ~/.Xilinx/Vivado
> sudo chmod -R 777 ~/.Xilinx/Vivado
> sudo chgrp -R $USER ~/.Xilinx/Vivado
```
14. Add the Vivado binary directory to your path:
```shell
> echo 'export PATH="/tools/Xilinx/Vivado/2020.1/bin:/tools/Xilinx/Vitis/2020.1/bin:$PATH"' >> ~/.bashrc
```
15. The ZCU106 board requires a Vivado license, so specify the license server using ``XILINXD_LICENSE_FILE``.  ``PORT`` and ``HOST`` should be replaced with appropriate values.
```shell
> echo 'export XILINXD_LICENSE_FILE="PORT@HOST"' >> ~/.bashrc
```
16. Source your shell configuration to pick up these changes:
```shell
> source ~/.bashrc
```
17. Test Vivado
```shell
> vivado -version
Vivado v2020.1 (64-bit)
SW Build 2902540 on Wed May 27 19:54:35 MDT 2020
IP Build 2902112 on Wed May 27 22:43:36 MDT 2020
Copyright 1986-2020 Xilinx, Inc. All Rights Reserved.
```

## Add a UDEV rule for the USB UART

Run the following commands to make the USB UART accessible to non-root users.  Note that this is a fairly broad rule; it makes all devices matching the pattern ``/dev/ttyUSB*`` available to all users.

```shell
> echo 'KERNEL=="ttyUSB[0-9]*",MODE="0666"' | sudo tee -a /etc/udev/rules.d/99-ftdi.rules > /dev/null
> sudo udevadm control --reload-rules
```
