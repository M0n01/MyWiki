#nfs #file #fichier #partage

C'est un protocole pour faire du partage de fichiers réseau.

Installer NFS
```bash
sudo apt install nfs-kernel-server -y
```

Check le status
```bash
systemctl status nfs-kernel-server
```

Se configure via `/etc/exports`.

|   |   |
|---|---|
|`rw`|Gives users and systems read and write permissions to the shared directory.|
|`ro`|Gives users and systems read-only access to the shared directory.|
|`no_root_squash`|Prevents the root user on the client from being restricted to the rights of a normal user.|
|`root_squash`|Restricts the rights of the root user on the client to the rights of a normal user.|
|`sync`|Synchronizes the transfer of data to ensure that changes are only transferred after they have been saved on the file system.|
|`async`|Transfers data asynchronously, which makes the transfer faster, but may cause inconsistencies in the file system if changes have not been fully committed.|

Créer un partage NFS
```bash
mkdir nfs_sharing
echo '/home/cry0l1t3/nfs_sharing hostname(rw,sync,no_root_squash)' >> /etc/exports
cat /etc/exports | grep -v "#"
```

Si nous avons créé un partage NFS et que nous souhaitons l'utiliser sur le système cible, nous devons d'abord le monter.
```bash
mkdir ~/target_nfs
mount 10.129.12.17:/home/john/dev_scripts ~/target_nfs
tree ~/target_nfs
```

Nous avons donc monté le partage NFS (**dev_scripts**) de notre cible (**10.129.12.17**) localement sur notre système dans le point de montage **target_nfs** sur le réseau et pouvons afficher le contenu comme si nous étions sur le système cible. 

