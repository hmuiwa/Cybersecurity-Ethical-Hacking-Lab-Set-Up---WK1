# VirtualBox Kali Linux Cybersecurity Lab

## Overview

This project documents the setup of a virtual cybersecurity testing lab using Oracle VirtualBox and Kali Linux. The lab provides an environment for learning networking, Linux administration, ethical hacking, penetration testing fundamentals, and Capture The Flag (CTF) exercises.

The initial setup uses Kali Linux as the attacking machine and a custom VirtualBox NAT Network to provide network connectivity and internet access. Additional virtual machines can be introduced later for controlled security testing.

## Objectives

- Install Oracle VirtualBox on a Windows host.
- Import and configure a Kali Linux virtual machine.
- Create a custom NAT Network using the `10.0.0.0/24` subnet.
- Configure Kali Linux with the static IP address `10.0.0.2/24`.
- Enable internet connectivity for Kali Linux.
- Enable shared clipboard, drag-and-drop, and a shared host folder.
- Test network connectivity and save a clean virtual machine snapshot.
- Prepare the environment for future ethical hacking and CTF exercises.

## Lab Environment

### Host Machine

- Operating System: Windows 10/11
- Virtualization Platform: Oracle VirtualBox
- Recommended RAM: 8 GB or more
- Recommended Storage: 256 GB SSD or more
- Processor: Intel Core i3/i5 or equivalent with virtualization support

### Virtual Machine

- Operating System: Kali Linux
- Role: Attacking and security testing machine
- Network Mode: NAT Network
- Static IPv4 Address: `10.0.0.2/24`
- Shared Host Directory: `/downloads`

## Network Architecture

The lab uses a custom VirtualBox NAT Network with the subnet `10.0.0.0/24`.

| Component | Configuration |
|---|---|
| Network | Custom NAT Network |
| IPv4 subnet | `10.0.0.0/24` |
| Kali Linux IP | `10.0.0.2/24` |
| Example gateway | `10.0.0.1` |
| Example DHCP range | `10.0.0.2–10.0.0.99` |
| Network mask | `255.255.255.0` |

**Note:** The gateway, DHCP range, and static IP must be configured consistently. If Kali uses `10.0.0.2`, exclude that address from the DHCP pool to prevent IP conflicts. Confirm the actual gateway in VirtualBox's NAT Network settings.

## Prerequisites

Download the required software from the official websites:

1. [7-Zip](https://www.7-zip.org/download.html) — for extracting downloaded archives.
2. [Oracle VirtualBox](https://www.virtualbox.org/wiki/Downloads) — for creating and running virtual machines.
3. [Kali Linux](https://www.kali.org/get-kali/) — download a VirtualBox image or an appropriate installation image.

Use compatible software versions and verify downloaded images when checksums are provided.

## Phase 1: Build the Lab

### Step 1: Install 7-Zip

Download and install 7-Zip if it is needed to extract the Kali Linux archive.

### Step 2: Install VirtualBox

1. Download VirtualBox from the official website.
2. Run the installer.
3. Follow the installation instructions.
4. Launch VirtualBox after installation.

### Step 3: Create a NAT Network

1. Open VirtualBox.
2. Navigate to the Network settings. The exact menu location depends on the VirtualBox version.
3. Create a new NAT Network.
4. Set the network CIDR to `10.0.0.0/24`.
5. Enable DHCP if required for other virtual machines.
6. Configure a non-overlapping DHCP range if Kali will use a static address.
7. Save the configuration.

### Step 4: Import Kali Linux

1. Download the official Kali Linux VirtualBox image.
2. Extract the archive if necessary.
3. In VirtualBox, import the appliance or add the provided virtual machine, depending on the downloaded format.
4. Review the RAM, CPU, and storage settings.
5. Start the Kali Linux virtual machine.

### Step 5: Configure the Kali Network Adapter

1. Shut down Kali Linux.
2. Open the virtual machine's Settings.
3. Select Network.
4. Enable Adapter 1.
5. Attach it to the custom NAT Network created earlier.
6. Select the correct network name.
7. Save the settings and start Kali Linux.

### Step 6: Configure Kali Linux Networking

Identify the network interface and current connection:

```bash
ip -br addr
ip route
nmcli connection show
```

Configure a static address using NetworkManager. Replace `Wired connection 1` if the connection has a different name.

```bash
sudo nmcli connection modify "Wired connection 1" \
  ipv4.method manual \
  ipv4.addresses 10.0.0.2/24 \
  ipv4.gateway 10.0.0.1 \
  ipv4.dns "1.1.1.1 8.8.8.8"

sudo nmcli connection down "Wired connection 1"
sudo nmcli connection up "Wired connection 1"
```

The gateway `10.0.0.1` is an example. Verify the gateway supported by your actual NAT Network configuration before applying these settings.

Verify the configuration:

```bash
ip -br addr
ip route
```

### Step 7: Test Connectivity

Check the following in order:

```bash
ip route
ping -c 4 10.0.0.1
ping -c 4 1.1.1.1
getent hosts www.networkwalks.com
```

If the gateway address differs, replace `10.0.0.1` with the actual gateway.

Successful IP connectivity and successful DNS resolution confirm different parts of the connection. A blocked ping alone does not necessarily mean internet access is unavailable.

### Step 8: Enable Clipboard and Drag-and-Drop

1. Open the Kali virtual machine settings.
2. Navigate to General → Advanced, where supported.
3. Enable Shared Clipboard.
4. Enable Drag'n'Drop.
5. Start Kali Linux.

Install the matching VirtualBox Guest Additions or guest utilities if these features do not work. Availability and behavior depend on the guest configuration and VirtualBox version.

### Step 9: Configure the Shared Folder

1. Create a folder named `downloads` on the Windows host.
2. Open the Kali VM settings.
3. Navigate to Shared Folders.
4. Add the host folder.
5. Set the share name to `downloads`.
6. Enable Auto-mount and Make Permanent where available.
7. Start Kali Linux.

For a Linux guest, install the required guest utilities if needed. Access permissions may require adding the user to the `vboxsf` group and logging out and back in.

Treat the shared folder as a transfer boundary: do not place sensitive host files or untrusted files in it unnecessarily.

### Step 10: Take a Snapshot

After confirming the basic setup:

1. Shut down or save the virtual machine in a consistent state.
2. Open the Snapshots view in VirtualBox.
3. Create a snapshot named `Kali-Clean-Installation`.
4. Add a description recording the working network configuration.

The snapshot provides a convenient recovery point before future lab exercises.

## Phase 2: Expand the Lab (Optional)

Additional virtual machines can be connected to the same NAT Network for controlled exercises.

Examples include:

- Windows 10/11 test virtual machine
- Windows Server evaluation virtual machine
- Windows 7 legacy test virtual machine, isolated and used only when necessary
- Android emulator or compatible Android virtual machine

Assign each machine a unique IP address within `10.0.0.0/24`. Confirm that static addresses do not overlap with the DHCP pool.

Test connectivity between the machines and document the results. Use intentionally vulnerable systems or systems you own and have explicit permission to test.

## Troubleshooting

### Kali Linux has no internet access

1. Confirm that the NAT Network exists and is enabled.
2. Check that the Kali network adapter is attached to the correct NAT Network.
3. Verify the IP address, subnet mask, default gateway, and DNS settings.
4. Ensure that no other machine is using `10.0.0.2`.
5. Test the gateway, external IP connectivity, and DNS resolution separately.
6. Restart the network connection and, if necessary, VirtualBox.

For a reported issue affecting some VirtualBox 7 and Kali Linux 2026.1-or-newer configurations, the following workaround may help if the connection is named `Wired connection 1`:

```bash
sudo nmcli connection modify "Wired connection 1" ipv4.dad-timeout 0
sudo nmcli connection down "Wired connection 1"
sudo nmcli connection up "Wired connection 1"
```

This is a targeted workaround, not a universal fix. Confirm the connection name and diagnose the actual network configuration first.

### Shared clipboard or drag-and-drop does not work

- Verify that the relevant VirtualBox settings are enabled.
- Check that compatible Guest Additions or guest utilities are installed.
- Restart the guest after installation.
- Check for version compatibility issues.

### Shared folder is inaccessible

- Confirm the host folder path and share name.
- Verify that the shared folder is mounted inside Kali.
- Check membership in the `vboxsf` group when applicable.
- Log out and back in after changing group membership.

## Security and Ethical Use

This lab is intended for education, authorized security testing, and CTF practice.

- Test only systems you own or have explicit permission to assess.
- Use intentionally vulnerable targets for exploitation practice.
- Keep vulnerable legacy operating systems isolated.
- Do not bridge vulnerable targets directly onto untrusted networks.
- Take snapshots before making major changes.
- Keep host files and credentials out of shared folders unless required.

## Progress Checklist

- [ ] Install VirtualBox
- [ ] Create the `10.0.0.0/24` NAT Network
- [ ] Import Kali Linux
- [ ] Configure the Kali static IP
- [ ] Verify internet and DNS access
- [ ] Enable clipboard and drag-and-drop
- [ ] Configure the `/downloads` shared folder
- [ ] Take the clean installation snapshot
- [ ] Capture screenshots and document test results
- [ ] Add optional lab machines

## References

- [Oracle VirtualBox Downloads](https://www.virtualbox.org/wiki/Downloads)
- [VirtualBox User Manual](https://www.virtualbox.org/manual/)
- [Kali Linux Downloads](https://www.kali.org/get-kali/)
- [7-Zip Downloads](https://www.7-zip.org/download.html)
- [NetworkWalks](https://www.networkwalks.com/)

## Disclaimer

This repository documents a personal learning environment. All testing should be conducted in authorized environments and in accordance with applicable laws and rules.
