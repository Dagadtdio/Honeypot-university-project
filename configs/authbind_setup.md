# Authbind setup for Cowrie on port 22

Cowrie was run as the dedicated `cowrie` Linux user, not as root. Since port 22 is a privileged port, `authbind` was used to allow only the `cowrie` user to bind to that port.

## Install authbind

```bash
sudo apt update
sudo apt install authbind -y
```

## Allow the cowrie user to bind port 22

```bash
sudo touch /etc/authbind/byport/22
sudo chown cowrie:cowrie /etc/authbind/byport/22
sudo chmod 500 /etc/authbind/byport/22
```

Check:

```bash
ls -l /etc/authbind/byport/22
```

Expected owner/group:

```text
cowrie cowrie
```

## Start Cowrie with authbind

```bash
sudo su - cowrie
cd /home/cowrie/cowrie
source cowrie-env/bin/activate
export PYTHONPATH=/home/cowrie/cowrie/src
AUTHBIND_ENABLED=yes cowrie start
```

## Verify

```bash
cowrie status
ss -tulpn | grep ':22'
```
