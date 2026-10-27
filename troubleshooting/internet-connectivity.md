Troubleshooting
Kali Linux has no internet access
Confirm that the NAT Network exists and is enabled.
Check that the Kali network adapter is attached to the correct NAT Network.
Verify the IP address, subnet mask, default gateway, and DNS settings.
Ensure that no other machine is using 10.0.0.2.
Test the gateway, external IP connectivity, and DNS resolution separately.
Restart the network connection and, if necessary, VirtualBox.
For a reported issue affecting some VirtualBox 7 and Kali Linux 2026.1-or-newer configurations, the following workaround may help if the connection is named Wired connection 1:

sudo nmcli connection modify "Wired connection 1" ipv4.dad-timeout 0
sudo nmcli connection down "Wired connection 1"
sudo nmcli connection up "Wired connection 1"
This is a targeted workaround, not a universal fix. Confirm the connection name and diagnose the actual network configuration first.

Shared clipboard or drag-and-drop does not work
Verify that the relevant VirtualBox settings are enabled.
Check that compatible Guest Additions or guest utilities are installed.
Restart the guest after installation.
Check for version compatibility issues.
Shared folder is inaccessible
Confirm the host folder path and share name.
Verify that the shared folder is mounted inside Kali.
Check membership in the vboxsf group when applicable.
Log out and back in after changing group membership.
