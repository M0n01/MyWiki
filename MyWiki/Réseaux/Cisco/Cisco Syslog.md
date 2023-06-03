
## Configurer l'horodatage Syslog

Par défaut, les messages de journal ne sont pas horodatés. .Par exemple, dans la figure, l'interface GigabitEthernet 0/0 de R1 est arrêtée. Le message journalisé dans la console n'identifie pas la date/l'heure de changement de l'état de l'interface. Les messages de journal doivent être horodatés de sorte que lorsqu'ils sont envoyés à une autre destination, comme un serveur Syslog, il reste une trace du moment où le message a été généré.

```
R1# configure terminal
R1(config)# interface g0/0/0
R1(config-if)# shutdown
R1(config-if)# exit
R1(config)# service timestamps log datetime
R1(config)# interface g0/0/0
R1(config-if)# no shutdown
R1(config-if)#
```

Utilisez la commande **service timestamps log datetime** pour forcer les événements journalisés à afficher la date et l'heure. Comme le montre la figure, lorsque l'interface GigabitEthernet 0/0/0 de R1 est de nouveau activée, les messages de journal contiennent maintenant la date et l'heure.

>[!Remarque]
>Si vous utilisez le mot clé **datetime** , vérifiez que l'horloge du périphérique réseau est définie manuellement, ou via NTP

