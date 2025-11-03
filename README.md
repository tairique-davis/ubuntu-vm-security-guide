# Ubuntu 24.04 Virtual Machine Setup & Basic Security Hardening

This guide provides step-by-step instructions for creating and securing an Ubuntu 24.04 virtual machine using VirtualBox. 

---
##  Objectives

- Demonstrate knowledge in key virtualization concepts
- Enable and verify hardware-assisted virtualization (SVM) in BIOS
- Install and Configure a Type 2 Hypervisor
- Create, configure, and install Ubuntu 24.04.3 LTS as a virtual machine
- Set up a safe and stable environment for learning Linux and future cybersecurity labs with basic security hardening

---
## ✅ Prerequisites

Before you begin, download the following:

1. **Ubuntu 24.04.3 LTS ISO**  
   https://ubuntu.com/download/desktop

2. **VirtualBox 7.2.4 for Windows**  
   https://www.virtualbox.org/wiki/Downloads

> **Note:** VirtualBox may require additional dependencies such as Python Core and win32api bindings depending on your system.
---
##  Background Concepts

###  What is Virtualization?

Virtualization allows you to run a separate operating system (called a virtual machine or VM) inside your computer. This provides a safe environment for testing tools, learning Linux, and performing cybersecurity exercises without affecting your main system.

For this project, we will use VirtualBox to run Ubuntu 24.04 as a virtual machine.

### Hypervisors

| Hypervisor Type | Description | Example |
|-----------------|--------------|----------|
| **Type 1 (Bare Metal)** | Runs directly on system hardware | VMware ESXi, Microsoft Hyper-V |
| **Type 2 (Hosted)** | Runs on top of an existing OS | VirtualBox, VMware Workstation |

This lab uses **VirtualBox**, a Type 2 hypervisor.

---
##  Lab Environment

| Component | Version / Details |
|-----------|------------------------|
| Host OS | Windows 11 |
| Hypervisor | Oracle VirtualBox 7.x |
| Guest OS | Ubuntu 24.04.3 LTS |
| CPU | AMD with SVM support |
| Recommended Specs | 8GB RAM, 4 cores, 30GB storage |
---
## Steps Deployed

### Step 1: Enable Virtualization in BIOS

Before installing, ensure virtualization is enabled on your system.

- My system uses an AMD chip so I went to the bios and checked that Secure Virtual Machine (SVM) mode was set to “enabled” (For Intel users this may be called Intel VT-x)
<p>
  <img src="https://github.com/user-attachments/assets/333fb57f-b143-42ed-bddb-da49ec0caed7" width=40% />
</p>

---

###  Step 2: Install VirtualBox

- Run the VirtualBox installer and install to your preferred file location.  
- Select default options unless you have a specific need to customize.

<p float="left">
  <img src="https://github.com/user-attachments/assets/beef7191-6d00-4526-8156-0492937c6364" width="30%" />
  <img src="https://github.com/user-attachments/assets/b9c1e13a-39ea-45c7-b9ff-c969ba3eb209" width="30%" />
  <img src="https://github.com/user-attachments/assets/0820ccf1-d735-4a8d-b8d5-2da240b55153" width=30%   />
</p>

---

###  Step 3: Download Ubuntu ISO

- Visit Ubuntu’s official website and download the latest LTS ISO. My computer uses an AMD chip with 64 bit architecture so I downloaded the version that worked with my architecture.

- If unsure of your system architecture:  
**Windows Settings → System → About → System Type**
<p>
  <img src="https://github.com/user-attachments/assets/c5920660-ef92-4e74-aa78-f7fa7f4dfcc8" width=45% />
</p>
---

### **Step 4: Create a New Virtual Machine**

- Open VirtualBox and Create a new Virtual Machine (VM)
<p>
  <img src="https://github.com/user-attachments/assets/18a8acb0-41e2-45ea-91c2-8b8677bef198" width=45% />
</p>

---

### **Step 5: Configure Your Virtual Machine**

#### **5.1 Name Your VM**

Give your VM a recognizable name such as `Linux-VM`.

#### **5.2 Select the Ubuntu ISO**

Click the ISO dropdown and browse to the ISO file downloaded earlier.
<p>
  <img src="https://github.com/user-attachments/assets/5c684990-bb5f-489e-9212-979a2a625c5b" width=45% />
</p>

#### **5.3 (Optional) Configure Unattended Installation**

Input the following if you want VirtualBox to automate the OS setup:

- Username
- Password
- Hostname
- Domain (optional)
- <p>
  <img src="https://github.com/user-attachments/assets/cebe23ee-d8ad-421f-952c-3aaeacf37dea" width=45% />
</p>

#### **5.4 Allocate System Resources**

Specify the amount of resources you will allocate to the virtual machine. For my system I have 32gb of memory and 16 cores on my CPU so I chose to allocate 4gb of memory and 4 cores from my host machine to the Guest VM. As a general rule of thumb don't exceed over half of your Host's resources

**Example (My Host Specs):**

| Host Specs | VM Allocation |
|------------|----------------|
| 32 GB RAM | 4 GB RAM |
| 16 Cores | 4 Cores |

If unsure, open **Task Manager → Performance** to view your usual usage.
<p>
  <img src="https://github.com/user-attachments/assets/53fd9057-f263-4ee5-8661-3f2656c6fce7" width=45% />
</p>

#### **5.5 Set Storage Space**

- Select how much storage space from the Host Machine you are going to allocate to the VM. In my case, I am allocating 15.10 GB to my VM. 
- In regards to Hard Disk File Type and Format, I chose to leave that as default VDI. For reference VirtualBox fully supports VDI, VHD, VMDK.
- By defaut, VirtualBox dyanically allocates selected virtual disk space meaning although I've chosen to allocate 15GB the virtual machine will only use as much space as needed growing up to a maximum size of 15GB.
<p>
  <img src="https://github.com/user-attachments/assets/2062133a-b24a-427a-bf3b-e1c30ece6dbb" width=45% />
</p>

---

### **Step 6: Start the VM**

Click **Start** to boot the VM for the first time. Ubuntu will install — this may take a few minutes.
<p>
  <img src="https://github.com/user-attachments/assets/b1bb1d6c-bcfb-498a-9d44-bd1b8642e1c8" width=45% />
</p>

---

##  Step 7: Update the System

After logging in, open the Terminal and run:

```bash
sudo apt update && sudo apt upgrade -y
```

This installs the latest security patches and software updates.

---

##  Step 8: Create a Non-Root User (If Needed)

If you did not create a non-root user during installation, create one now:

```bash
sudo adduser cyberlab
sudo usermod -aG sudo cyberlab
```

This creates a user named `cyberlab` and grants administrator privileges.

---

##  Step 9: Enable the Firewall

Install and enable UFW (Uncomplicated Firewall):

```bash
sudo apt install ufw -y
sudo ufw enable
sudo ufw status
```

UFW helps protect your system from unwanted network access.

---

##  Step 10: Take a Snapshot of VM

A snapshot allows you to save the current state of your VM. You can return to this clean, secure version at any time.

1. Shut down the VM.
2. In VirtualBox, select the VM.
3. Go to **Snapshots**.
4. Click **Take Snapshot**.
5. Name it: `linux_vm_secure`

---
## 🧩 Optional: Additional Security Hardening

The following steps provide extra security.

### A. Secure SSH Access

SSH allows encrypted remote access to the VM. We will harden SSH to reduce the attack surface and protect the confidentiality, integrity, and availability of the Virtual Machine.

SSH is useful for the following reasons:
- Installing SSH allows secure, encrypted remote administration of the VM
- Disable root login: Root has unrestricted control. Consider disabling root login to follow the Principle of Least Privilege (PoLP) which defines that users only get the minimum permissions needed to execute tasks.
- Disable password login: Passwords are more susceptible to brute-force attacks and credential theft.SSH key-based authentication uses public-key cryptography to provide stronger authentication

#### Install and enable SSH
```bash
sudo apt install openssh-server -y
sudo systemctl enable ssh
sudo systemctl status ssh
```

#### Edit SSH configuration:

```bash
sudo nano /etc/ssh/sshd_config
```

#### Set the following values to disable Root Login:

```
PermitRootLogin no      #disables direct root login to enforce PolP
PasswordAuthentication no
```

#### Save and apply changes:

```bash
sudo systemctl restart ssh   #restarts SSH to apply the new security config
```
---
Your Virtual Machine is now ready for use as a safe environment to learn Linux and begin practicing cybersecurity!
