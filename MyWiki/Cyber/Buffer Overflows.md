
Les dépassements de tampon sont devenus moins courants dans le monde d'aujourd'hui, car les compilateurs modernes ont intégré des protections de mémoire qui rendent difficile l'apparition accidentelle de bogues de corruption de mémoire.

Ces attaques ne se limitent pas aux fichiers binaires ; un grand nombre de débordements de tampon se produisent dans les applications Web, en particulier les appareils embarqués qui utilisent des serveurs Web personnalisés.

La cause la plus importante des dépassements de tampon est l'utilisation de langages de programmation qui ne surveillent pas automatiquement les limites de la mémoire tampon ou de la pile pour empêcher un dépassement de tampon (basé sur la pile). Il s'agit notamment des langages C et C++, qui mettent l'accent sur les performances et ne nécessitent pas de surveillance.