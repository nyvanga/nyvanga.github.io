# Time

"Time is an illusion. Lunchtime doubly so." (Douglas Adams)

## Time is drifting on every reboot

To check if something is wrong, run:

```
date
hwclock --show
```

And compare the results.

If they differ, update your system clock first:

```
ntpdate <server address>
```

And then the hardware clock with information from the system clock:

```
hwclock --systohc
```

Reboot the computer and check again.

Without knowing why, I had to do this twice before it fixed the problem.

Maybe something to do with the system setting the hardware clock and shutdown, and reading on boot.
