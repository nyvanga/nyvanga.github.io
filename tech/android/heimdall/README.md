# Flash recovery TWRP on Samsung devices using heimdall

Download: [heimdall](https://glassechidna.com.au/heimdall/)

Get the Team Win Recovery Project (TWRP) image for your device.

Run command ([source](https://xdaforums.com/t/how-to-flash-recovery-using-linux-and-heimdall-solved.4238595/)):
```
heimdall flash --RECOVERY recovery.img --no-reboot
```

Will output something like:
```
Heimdall v1.4.0

Copyright (c) 2010-2013, Benjamin Dobell, Glass Echidna
http://www.glassechidna.com.au/

This software is provided free of charge. Copying and redistribution is
encouraged.

If you appreciate this software and you would like to support future
development please consider donating:
http://www.glassechidna.com.au/donate/

Initialising connection...
Detecting device...
Claiming interface...
Setting up interface...

Initialising protocol...
Protocol initialisation successful.

Beginning session...

Some devices may take up to 2 minutes to respond.
Please be patient!

Session begun.

Downloading device's PIT file...
PIT file download successful.

Uploading RECOVERY
0%..4%..8%..12%..16%..20%..24%..28%..32%..36%..41%..45%..49%..53%..57%..61%..65%..69%..73%..77%..82%..86%..90%..94%..98%..100%
RECOVERY upload successful

Ending session...
Releasing device interface...

```
(Screen never said completed on Samsung Galaxy Tab S3, but the above session disconnected from the device)

When the above completes, start holding down the recovery mode keys.

This is important as per document "NOTE: Be sure to reboot into recovery immediately after installing the custom recovery. If you don’t the stock ROM will overwrite the custom recovery with the stock recovery, and you’ll need to flash it again."
