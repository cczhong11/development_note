
# Syncthing

## install on linux

```bash
curl -s https://syncthing.net/release-key.txt | gpg --dearmor | sudo tee /usr/share/keyrings/syncthing-archive-keyring.gpg >/dev/null
echo "deb [signed-by=/usr/share/keyrings/syncthing-archive-keyring.gpg] https://apt.syncthing.net/ syncthing stable" | sudo tee /etc/apt/sources.list.d/syncthing.list
sudo apt-get update
sudo apt-get install syncthing
```

## start syncthing

```bash
syncthing
```

## open port 8384

in webui or `sudo ufw allow 8384/tcp`

## add syncthing to autostart

```bash
sudo systemctl enable syncthing@$USER.service
```

## allow device

in webui
