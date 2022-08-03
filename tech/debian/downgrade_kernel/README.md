# Downgrade kernel on Debian

First check which versions are installed:
```
sudo apt list --installed | grep linux-image
```

Returns something like:
```
linux-image-5.10.0-15-amd64/stable-security,now 5.10.120-1 amd64 [installed,automatic]
linux-image-5.10.0-16-amd64/stable,now 5.10.127-1 amd64 [installed,automatic]
linux-image-amd64/stable,now 5.10.127-1 amd64 [installed]
```

Now choose which version you want to get rid of.

Typically it is the newest, since it is often a case of "kernel upgrade broke this kernel module..."

Remove with:
```
sudo apt remove linux-image-5.10.0-16-amd64
```

Now if this is the running kernel, the install will warn you that "This can make the system unbootable", and prompt you to "Abort kernel removal?".

But as long as there is another WORKING kernel installed, it is no problem. So go ahead and choose "No" to this.

After the remove check versions again, and it should return:
```
linux-image-5.10.0-15-amd64/stable-security,now 5.10.120-1 amd64 [installed,automatic]
```

Notice that the `linux-image-amd64` meta package is gone as-well.

This will ensure that the kernel is not updated automatically.

And so if you want to re-enable automatic updates of the kernel, simply reinstall with:

```
sudo apt install linux-image-amd64
```