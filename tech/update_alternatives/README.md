# Update alternatives

To alter the "alternative" of one:
```
update-alternatives --config <name>
```

Fx.:
```
update-alternatives --config editor
```

List alternatives:
```
update-alternatives --list <name>
```

Fx.:
```
$ update-alternatives --list editor
/bin/ed
/bin/nano
/usr/bin/mcedit
/usr/bin/vim.tiny
```

Set an alternative directly:
```
update-alternatives --set editor /usr/bin/vim.tiny
```