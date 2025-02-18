# BPI-R3-R4 Image Builder

A project to create custom **OpenWRT** images for **Banana Pi R3 and R4** using **ImageBuilder**, fully automated with **Vagrant** and **Ansible**. This setup automates the provisioning of a virtualized build environment, making the process of building images efficient and reproducible.

## Features

- **Automated image creation** for Banana Pi R3 and R4
- **Uses Vagrant & Ansible** to automate the build process
- **libvirt** as the virtualization provider, with example on how to install it on **Fedora**.
- ✅ Tested on Fedora 41
- **Customizable images** based on your needs, the installed and removed packages are in the `packages.txt` file
- **Reproducible builds** with minimal setup

## Prerequisites

- **Ansible** (version 2.18.2 but earlier versions may work)
- **Vagrant** (version 2.4.3 but earlier versions may work)
- **libvirt** (version 10.6.0 but earlier versions may work)

To use this project, ensure you have the following installed on your **Fedora** system:

### **1. Install Vagrant**

```bash
sudo dnf install -y vagrant
```

### **2. Install Libvirt and Required Dependencies**

```bash
sudo dnf install -y @virtualization
sudo systemctl enable --now libvirtd
sudo usermod -aG libvirt $(whoami)
newgrp libvirt
```

### **3. Install the Vagrant Libvirt Plugin**

```bash
vagrant plugin install vagrant-libvirt
```

### **4. Install Dependencies**

Before proceeding, ensure Python 3 is installed. Then, install Ansible and required dependencies with:

```bash
pip install -U -r requirements.txt
```

## Installation

### **1. Clone the Repository**

```bash
git clone https://github.com/svilcu/bpi-r3-r4-image-builder.git
cd bpi-r3-r4-image-builder
```

### **2. Start the Vagrant Environment**

```bash
vagrant up
```

Running `vagrant up` will:
- Start a VM using libvirt
- Use Ansible to configure the build environment
- Generate a custom Banana Pi R3/R4 image and place it in a folder on the local machine.

### **3. Degugging: SSH into the VM**

```bash
vagrant ssh
less /tmp/output # View the image build logs
```

## Usage

After the build completes, the generated image will be saved as:

```bash
../images/r3-custom.itb  # For R3  
../images/r4-custom.itb  # For R4  
```

After you copied the image on your OpenWRT machine you can upgrade with:

```bash
sysupgrade -n /path/to/r4-custom.itb
```

## Configuration

In the `playbook.yaml` file in the `vars` section you may change:

```yaml
  board: "r4"                # Board model: "r3" or "r4"
  destination: "../images"    # Local folder where the built image is stored
  openwrt_version: "24.10.0"  # OpenWRT version (24.10.0 required for R4)
```

Older than `24.10.0` versions of OpenWRT will not work with the `R4` variant of the board.
Add or remove packages in the `packages.txt` file according to your needs.
I do not recommend changing the box type in the Vagrant file since the playbook is customized for a **Fedora** box.
If you use a different virtualization provider (e.g., VirtualBox), update the Vagrantfile accordingly

## Troubleshooting

### **Check Vagrant and Libvirt Status**

```bash
# Check Vagrant status
vagrant status  

# Verify that libvirt is running
sudo systemctl status libvirtd
```

### **Restart Libvirt if Needed**

```bash
sudo systemctl restart libvirtd
```

### **Force rebuild the VM** (if issues arise)

```bash
vagrant destroy -f && vagrant up
```

### **Cleanup**

```bash
# Destroy the VM without affecting generated images
vagrant destroy -f
```

## License

This project is licensed under the MIT License. See `LICENSE` for details.

## Contributions

Contributions are welcome! Feel free to submit issues or pull requests.

## Author

[Stefanita Vilcu](https://github.com/svilcu)

## Documentation

[OpenWRT Imagebuilder](https://openwrt.org/docs/guide-user/additional-software/imagebuilder)
