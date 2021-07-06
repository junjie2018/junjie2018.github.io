sudo systemctl disable systemd-resolved
sudo systemctl stop systemd-resolved

sudo systemctl enable systemd-resolved
sudo systemctl start systemd-resolved