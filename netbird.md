# Install and configure netbird

git clone https://aur.archlinux.org/netbird.git
cd netbird/
makepkg -si
sudo systemctl restart netbird@cloud.service

## Install completions for bash

```bash
rw
timedatectl set-timezone Europe/Paris
export SETUP_KEY=KEY_FROM_NETBIRD_ACCOUNT

pacman -Syu
useradd --groups wheel --create-home netbird
visudo /etc/sudoers
passwd netbird

pacman -S bash-completion
netbird completion bash > /etc/bash_completion.d/netbird.bash

netbird service uninstall
systemctl enable netbird@cloud.service --now
systemctl daemon-reload

## In case this does not survice an update -> Might replace with a full override:
## systemctl edit --full netbird
## And add the content of https://raw.githubusercontent.com/netbirdio/netbird/main/release_files/systemd/netbird%40.service or `/usr/lib/systemd/system/netbird@.service`
## Replace all %i with a name (cloud, selfhost server, etc.)
## More info at https://askubuntu.com/a/659268

netbird up --setup-key ${SETUP_KEY} --allow-server-ssh
ro
```