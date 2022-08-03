# Bitlocker

[How to Use BitLocker Without a Trusted Platform Module (TPM)](https://www.howtogeek.com/6229/how-to-use-bitlocker-on-drives-without-tpm/)

## Switching BitLocker protection methods without re-encrypting

[Source. Not quite up to date.](https://www.stevenmaude.co.uk/posts/switching-bitlocker-protection-methods-without-re-encrypting)

In an admin prompt, to check "protectors" status:
```
manage-bde -protectors -get <drive>

```

Remove TMP (leaves recovery key intact):
```
manage-bde -protectors -delete <drive> -type TPM

```

Add password protector:
```
manage-bde -protectors -add <drive> -password

```
It might complain that:
```
ERROR: An error occurred (code 0x8031006a):
Group Policy settings do not permit the creation of a password.
```
If that is the case, follow the howto geek above to enable that policy.
