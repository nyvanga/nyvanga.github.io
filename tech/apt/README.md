# Apt

## Install .deb file the "nice way"

Use `gdebi` which will also resolve and install its dependencies:
```
apt-get install gdebi-core

gdebi <.deb file>
```

## Export a key

Find the key:
```
apt-key adv --list-keys
```

Get the id and export as GPG with:
```
apt-key adv --output <output filename, - is stdout> --export <key id>
```

## Delete a key

Find the key:
```
apt-key adv --list-keys
```

Get the id and delete with:
```
apt-key adv --delete-keys <key id>
```

## Apt troubleshooting

[Apt-get stuck at 0% [Working]](https://askubuntu.com/questions/498462/apt-get-stuck-at-0-working)
