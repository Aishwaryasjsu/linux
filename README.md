
# Work Distribution:
## Aishwarya 
Creation of the eax = 0x4FFFFFFC on gcp <br>
Alteration of cpuid.c for the implementation of the feature as per the specification of the assignment. <br>
cpuid package installation for the nested VM <br>
Verification and Validation of Results. (Collaboration with Srinishaa) <br>

## Srinishaa
Creation of eax = 0x4FFFFFFD on gcp <br>
Alteration of vmx.c for implementing the feature as per the specification of the assignment. <br>
Verification of Results. <br>
Documentation (Collaboration with Aishwarya). <br>

## Prerequisite:
Assignment 1 setup

# Setup of gcc:
gcloud compute instances create [YOUR_INSTANCE_NAME] --machine-type=n2-standard-8 --boot-disk-size=200 --enable-nested-virtualization
Enable nested virtualisation 
 We are enabling nested virtualization by adding the flag "--enable-nested-virtualization".

### SSH into linux:
gcloud compute ssh --zone "YOUR_ZONE" "YOUR_INSTANCE_NAME" --project "YOUR_PROJECT_NAME"


### Install GCC:
sudo apt install gcc make

### Git clone repo from torvald/linux
git clone https://github.com/aishwaryasjsu/linux

### Building source tree
sudo cp /boot/config-5.10.0-19-cloud-amd64 .config
Run command as below:
make oldconfig
For architecture:
make prepare
make -j 8 modules
make -j 8
sudo make INSTALL_MOD_STRIP=1 modules_install
sudo make install
sudo reboot
Now you should be able to see your updated linux version when you run the below command
      uname -a 
      ![image](https://user-images.githubusercontent.com/111553278/205859704-5e7c7302-1a3a-41d8-9b5f-006b1f9d70a7.png)

 
### Install virt-install
virt-install is a command line tool to provision new virtual machines
Create a nested virtual machine
sudo virt-install --name ubuntu-1 --os-variant ubuntu20.04 --vcpus 2 --ram 2048 --location http://ftp.ubuntu.com/ubuntu/dists/focal/main/installer-amd64/ --network bridge=virbr0,model=virtio --graphics none --extra-args='console=ttyS0,115200n8 serial'

### List your virtual machines
virsh list --all

### Access your nested virtual machine through the console
sudo virsh  console ubuntu-guest

### STEPS TO BE FOLLOWED:
The file vmx.c and cpuid.c should be altered as per the specification required in the assignment.
Alter the code in the file “vmx.c” at KVM with the path /linux/arch/x86/kvm/vmx/vmx.c.
Alter the code in the file “cpuid.c” at KVM with the path /linux/arch/x86/kvm/cpuid.c.

In the cpuid.c file for the node eax = 0x4FFFFFFC:
The total number of exits should be returned in the eax.
In the cpuid.c file for the node eax = 0x4FFFFFFD:
The exit number provided for the count of exits in ecx. Then the output will be returned to  eax.
Upon the alterations, for the effect of the changes made we should run the commands that is shown below for the kernel.
 make -j 8 modules 
Then we should run this command.
 make  -j 8
After that, we should run the below command.
sudo make INSTALL_MOD_STRIP=1 modules_install.
As a next step check if the kvm module is already loaded with the command:
lsmod | grep kvm


If it’s already loaded remove kvm using the below command:

 
sudo rmmod kvm_intel
sudo rmmod kvm
sudo modprobe kvm
sudo modprobe kvm_intel
sudo virsh  start  ubuntu-guest


### Verification Process:
For verifying the code, installation of the package cpuid should be performed on the nested VM.
Use the below commands:
Sudo apt-get update
Sudo apt-get  install cpuid

### Verification of 0x4FFFFFFC:
cpuid  -1 -l 0x4ffffffc
Verification of 0x4FFFFFFD:
cpuid -1 -l 0x4ffffffd

![image](https://user-images.githubusercontent.com/111553278/205860057-223ef10c-2102-415c-8e18-b3bece4605b9.png)

# ASSIGNMENT 3
Questions
1.Comment on the frequency of exits – does the number of exits increase at a stable rate? Or are there more exits performed during certain VM operations? Approximately how many exits does a full VM boot entail?
 we noticed that the frequency of exits increasing. There are more exits whenever a VM is booting. On testing we found that the total number of exits during a full VM boot is 956591. The total number of exits could be more or less.


2. Of the exit types defined in the SDM, which are the most frequent? Least?
The most frequent exit type is 48 and the least is 47, however, there are a lot of exit types which are not executed at all and their exit frequency is 0.
Output for exit reason 47

Minimum exit:
![image](https://user-images.githubusercontent.com/111553278/206949372-0df1012e-4fd2-462b-870a-9217bb77b7ee.png)

Maximun exit:
![image](https://user-images.githubusercontent.com/111553278/206949432-4035459b-5ba9-41b1-898b-d6dd7f4bad3a.png)



commands executed:
![image](https://user-images.githubusercontent.com/111553278/206949283-1902f0ad-980c-4aea-a1db-e66817df4cfb.png)


 
