# mac-spoof-persistence
This project provides a Bash-based automation to spoof the Wi-Fi MAC address on macOS at every system boot. This is useful for privacy testing, bypassing hardware-based tracking, and learning macOS system administration.

How it Works

The Script: A Bash script uses openssl to generate a random 12-digit hexadecimal string and applies it to the en0 interface via ifconfig.

Persistence: Uses a LaunchDaemon (.plist) to ensure the script executes with root privileges during the macOS boot sequence.

Project Structure

/usr/local/bin/spoof.sh - The logic script.

/Library/LaunchDaemons/com.user.spoof.plist - The automation trigger.

Key Skills Demonstrated

Bash Scripting & Automation

macOS System Integrity & Daemons

Network Security & Hardware Masking

Phase 1: Create the Automation Script
This script generates a random ID and applies it to your Wi-Fi (en0).

Create the folder (if it doesn't exist):

Bash

sudo mkdir -p /usr/local/bin
Create and write the script: (This command uses cat to write the file directly so you don't have to use an editor.)

Bash

sudo tee /usr/local/bin/spoof.sh <<'EOF'
#!/bin/bash
# Generate random MAC and apply to en0
new_mac=$(openssl rand -hex 6 | sed 's/\(..\)/\1:/g; s/.$//')
ifconfig en0 ether $new_mac
EOF
Make the script runnable:

Bash

sudo chmod +x /usr/local/bin/spoof.sh
Phase 2: Create the Boot Trigger (LaunchDaemon)
This tells macOS to run the script automatically every time the computer turns on.

Write the configuration file:

Bash

sudo tee /Library/LaunchDaemons/com.user.spoof.plist <<EOF
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>Label</key>
    <string>com.user.spoof</string>
    <key>ProgramArguments</key>
    <array>
        <string>/usr/local/bin/spoof.sh</string>
    </array>
    <key>RunAtLoad</key>
    <true/>
</dict>
</plist>
EOF
Set the correct security permissions:

Bash

sudo chown root:wheel /Library/LaunchDaemons/com.user.spoof.plist
Activate the service:

Bash

sudo launchctl load -w /Library/LaunchDaemons/com.user.spoof.plist
Phase 3: Verification & Convenience
Use these commands to check your work and make it easier to use later.

Check your current MAC address:

Bash

ifconfig en0 | grep ether
Create a "shortcut" command: (This lets you type newmac anytime to change it manually without rebooting.)

Bash

echo "alias newmac='sudo /usr/local/bin/spoof.sh'" >> ~/.zshrc && source ~/.zshrc
