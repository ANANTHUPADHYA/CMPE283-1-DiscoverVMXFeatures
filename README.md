# CMPE-1-DiscoverVMXFeatures
Discover VMX Features present in my processor (Intel)

### Functionality Implemented
-- Read the VMX configuration MSRs to ascertain support capabilities/features - Entry / Exit / Procbased / Secondary Procbased / Pinbased controls
-- Interpret and output values read from the MSR to the system via printk (..) for each group of controls above, including whether the value can be set or cleared.

The below procedure describes the steps for the same

1)      Clone the kernel sources from the master linux git repository:
> git clone https://github.com/torvalds/linux.git

This clones the linux kernel sources.
2)      Change to the cloned directory:
> cd linux

### Module development
3)      Create a new directory named “cmpe283” in the previously cloned “linux” source folder.
> mkdir cmpe283

4)      Copy the “Makefile” and cmpe283-1.c provided to the cmpe283 directory.

5)      The functionality to query all the other MSRs is explained below.
•       By referring to SDM, I created different structures with name (description) and bit positions for entry, exit, procbased, secondary procbased controls.
•       To detect and print VMX capabilities of CPU, the function report_capability( ) is called with right parameters passed in order to print pinbased, procbased, entry and exit controls.
•   To determine if true controls are available, a new function has been written to check the 55th  bit of IA32_VMX_BASIC MSR. If this bit is set, then true controls are available and another function is be called to print the corresponding true VMX capabilities.
•       To determine if Secondary Procbased controls are available, a new function is written to check 63rd bit of IA32_VMX_PROCBASED MSR. If this bit is set, then secondary procbased controls are available and another function will be called to print the corresponding secondary procbased capabilities.

6)      Change to the cmpe283 directory:
> cd cmpe283

7)      Build the module using below command inside the cmpe283 directory:
> make all

8)      Load and unload the module using below commands:
When a module is inserted into the kernel, the module_init macro would be invoked, which will call the function init_module.
Similarly, when the module is removed with rmmod, module_exit macro will be invoked, which will call the cleanup_module.
> sudo insmod cmpe283-1.ko

> sudo rmmod cmpe283-1.ko

9)      The VMX features gets logged in the kernel log and which can be verified using the dmesg command:
> dmesg
