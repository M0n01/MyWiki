
## Outils de Gestion de La Configuration

Les outils de gestion de la configuration utilisent les demandes d'API RESTful pour automatiser les tâches et peuvent évoluer sur des milliers de périphériques. Les outils de gestion de la configuration conservent les caractéristiques d'un système ou d'un réseau pour plus de cohérence. Voici quelques caractéristiques du réseau que les administrateurs bénéficient de l'automatisation:

- Logiciel et contrôle de version
- Attributs de périphérique tels que les noms, l'adressage et la sécurité
- Configurations de protocole
- Configuration des listes de contrôle d'accès

Les outils de gestion de la configuration incluent généralement l'automatisation et l'orchestration. L'automatisation est lorsqu'un outil exécute automatiquement une tâche sur un système. Cela peut être la configuration d'une interface ou le déploiement d'un VLAN. L'orchestration est le processus de la manière dont toutes ces activités automatisées doivent se produire, telles que l'ordre dans lequel elles doivent être effectuées, ce qui doit être achevé avant qu'une autre tâche ne commence, etc. 

Plusieurs outils sont disponibles pour faciliter la gestion de la configuration:

- Ansible
- Chef
- Puppet
- SaltStack

Ansible, Chef, Puppet et SaltStack sont tous livrés avec la documentation de l'API pour configurer les demandes d'API RESTful. Tous prennent en charge JSON et YAML ainsi que d'autres formats de données.

| Caractéristique                               | Ansible                                  | Chef                           | Puppet                   | SaltStack                |
| --------------------------------------------- | ---------------------------------------- | ------------------------------ | ------------------------ | ------------------------ |
| **Quel langage de programmation?**            | Python + YAML                            | Ruby                           | Ruby                     | Python                   |
| **Avec ou sans agent?**                       | Sans agent                               | Approche reposant sur un agent | Prend en charge les deux | Prend en charge les deux |
| **Comment les périphériques sont-ils gérés?** | Tout périphérique peut être “controller” | Chef Master                    | Puppet Master            | Salt Master              |
| **Qu'est-ce qui est créé par l'outil?**       | Playbook                                 | Cookbook                       | Manifest                 | Pillar                   |

