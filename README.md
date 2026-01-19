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
