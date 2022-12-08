Quand on  ne connait pas la taille du fichier -> cat, grep, less... mais pas nano
Pour fichier compressé (gzip) -> zcat, zgrep, zless

500 premiers octets < secteur de démarrage < le MBR (GRUB par exemple)

En MBR -> 4 partitions max

En EFI -> Plein de partitions primaire possible

Le BIOS va chercher le MBR pour trouver le disque de démarrage.

GRUB va ensuite chercher le /boot

initrd.img = première image de démarrage. C'est le noyau

Première chose que démarre Linux après le noyau -> /sbin/init

init = binaire fournit par systemd

## TP partie 4

#### page 3

Possible que si on est présent physiquement (pas de ssh).

Connection refused : Port pas en ecoute ou Reject dans iptable

```bash
ss -taupen     // voir les ports en ecoute
ss -tlpen    

systemctl status ssh.service    // voir ssh

systemctl enable ssh.service    // enable

systemctl start ssh.service   // démarrer

nano /etc/ssh/sshd_config     // fichier de conf de ssh (pour modifier le port par exemple ou activer authentification par mdp)

systemctl restart ssh    // redémarrer ssh

ls -lh .ssh/authorized_keys   // voir les droits sur le fichier ssh (important)

echo 'cle_publique' > .ssh/authorized_keys   // ajouter clé publique dans les clé autorisées

chown

cat /var/log/tomcat9/catalina.out      // fichier important dans tomcat

journalctl -xe      // affiche la fin et avec des explications

```

## Exam

- Apache
- MySQL
- Tomcat
- Xwiki
- PHPmyadmin
- Wordpress