# linux.md

## Show fingerprint of ~/.ssh/id_rsa.pub

```
ssh-keygen -E md5 -lf ~/.ssh/id_rsa.pub
```

 * `-E`: Specifies the hash algorithm used when displaying key fingerprints.  Valid options are: “md5” and “sha256”.  The default is “sha256”.
 * `-l`: Show fingerprint of specified public key file.
 * `-f`: Specifies the filename of the key file.
