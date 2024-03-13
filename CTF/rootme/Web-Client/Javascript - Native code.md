
En inspectant la page on peut voir un script qui est obfusqué.
Il faut le passer dans un "deobfuscator" pour le rendre lisible.

Cela donne ce script : 
```javascript
function anonymous( ) { a=prompt('Entrez le mot de passe');if(a=='toto123lol'){alert('bravo');}else{alert('fail...');} }
```

Flag = toto123lol

