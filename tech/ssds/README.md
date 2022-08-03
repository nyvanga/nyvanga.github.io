# SSD's

Good place to start [STOP Making These SSD Mistakes! Best SSD for Gaming 2021](https://youtu.be/Yc85SpwmNyk) (youtube).

Variants:
- SSD 2.5" via SATA
- M.2 via SATA
- M.2 via PCIe (nvme)

Beware that M.2 SATA looks much like nvme M.2, but performs like 2.5" SATA SSD's.

## Secure erasing SSD (or hard-drive)

[How to Securely Erase an SSD or HDD Before Selling It or Your PC](https://www.tomshardware.com/how-to/secure-erase-ssd-or-hard-drive)

From Windows use diskpart (probably best in recovery):
```
DISKPART> list disk
DISKPART> select disk <disk number from list>
DISKPART> clean all
```

## PCIe adapter cards for nvme

[PCIE x16 to 4x M.2 nvme adapter recommendations](https://www.reddit.com/r/HomeServer/comments/supo88/pcie_x16_to_4x_m2_nvme_adapter_recommendations/)

[Cooling an Overheating MVMe SSD](https://blog.insanegenius.com/2021/03/15/cooling-an-overheating-mvme-ssd/)

Motherboards may or may not support bifurcation of a x16 slot into 4 x4 slots, and some only support it in specific slots if some other slots are not populated.

[Compatibility of PCIE bifurcation between Hyper M.2 series Cards and Add-On Graphic Cards](https://www.asus.com/support/FAQ/1037507)

[ASUS HYPER M.2 X16 Card V2](https://www.asus.com/dk/Motherboards-Components/Motherboards/Accessories/HYPER-M-2-X16-CARD-V2/)

[ASUS HYPER M.2 X16 Gen 4 Card](https://www.asus.com/dk/Motherboards-Components/Motherboards/Accessories/HYPER-M-2-X16-GEN-4-CARD/)