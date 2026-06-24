# UFW rules used in the lab

The goal was to limit communication to the required paths only.

## VM addresses

```text
test-client:     10.50.0.20
cowrie-proxy:    10.50.0.10
backend-ubuntu:  10.50.0.30
```

## Cowrie VM

The Cowrie VM accepts SSH honeypot traffic from the test-client and can connect to the backend SSH service.

```bash
sudo ufw default deny incoming
sudo ufw default deny outgoing
sudo ufw allow from 10.50.0.20 to any port 22 proto tcp
sudo ufw allow out to 10.50.0.30 port 22 proto tcp
sudo ufw enable
sudo ufw status verbose
```

## Backend VM

The backend only accepts SSH from the Cowrie VM.

```bash
sudo ufw default deny incoming
sudo ufw allow from 10.50.0.10 to any port 22 proto tcp
sudo ufw enable
sudo ufw status verbose
```

## Test-client VM

The test-client was used as the attacker simulation machine. UFW was not required for the basic attack simulation, but if SSH file transfer to the test-client is needed, allow SSH only from trusted lab hosts.
