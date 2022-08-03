# Debian on a server

## Disable suspend and hibernate completely

```
sudo systemctl mask sleep.target suspend.target hibernate.target hybrid-sleep.target
```
