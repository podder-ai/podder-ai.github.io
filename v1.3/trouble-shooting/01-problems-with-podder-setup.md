# Problems with Podder setup.
## Cannot run virtual machine in VirtualBox (Windows10 Pro).
In Windows10 Pro, VirtualBox does not work successfully if the Hyper-V setting is enabled. From VirtualBox 6.x versions, Hyper-V support is implemented, but the problem in running virtual machine still occurs in v6.0.6. So we strongly recommend you to turn off Hyper-V settings when using Windows10 Pro.

Please make sure Hyper-V setting is disabled. In order to disable Hyper-V settings, please follow below instructions.
    1. From Start menu, open control panel.
    2. Select Programs.
    3. Select Turn Windows features on or off.
    4. Turn off Hyper-V option from the list shown.
