# Mise en place d'une ip static sur Linux via CLI <br/>
Modifier le fichier ```/etc/network/interfaces``` repere l'interface reseau et le modifié

```
nano /etc/network/interfaces
```
Modifier le fichier comme ceci
```
iface ens33 inet static
  address "IP/CIDR"
  gateway "Passerelle"
  dns-nameversers "ServeurDNS"
  dns-domain "NomDeDomaine"
```
Enregistrer > Quitter
