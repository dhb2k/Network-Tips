# Connection distante : A ←→B

---

Pour connecter à un serveur ouvert sur une machine, à l’aide d’une autre, il faut :

1. Se rassurer que les machine sont dans le même sous réseau (aidez vous des premiers chiffre de l’adresse ip, ils doivent être identique)
2. Vérifier que le service est lancé sur l’adresse ip de la VM1 et non sur les localhost
3. Ouvrir le navigateur de la VM2 et saisir

```bash
http://<ip_VM1>:port_VM1

# exemple :

http://192.168.1.50:8888
```

Si les machines ne sont pas dans le même sous réseau, une solution possible est le port forwarding.

## Port forwarding

Ici le but est de joindre un ordinateur distant connecté à un autre réseau. La première étape consiste à se connecter en VPN au réseau distant. Enfin on peut faire la redirection de port avec la commande ci-dessous :

```bash
ssh -L localhost:port_local:adresse_ip:port_distant user@bridge_ip_adress
```

On peut maintenant visualiser le serveur ouvert à partir du navigateur en saisissant dans la barre d’adresse :

```bash
http://localhost:port_local
```

## Exemple d’application du Port Forwarding

Nous avons installé Spark dans une machine virtuelle nommé tp-hadoop-29 (192.168.x.x). Et nous voulons pouvoir y accéder depuis notre localhost. Commencé par lancer le service Spark sur la machine virtuelle.

![Untitled](Connection%20distante%20A%20%E2%86%90%E2%86%92B%20f7beb932d2824b68a45e93060c2472cd/Untitled.png)

La session Spark est bien démarré et une Web UI est disponible. On peut donc démarrer la redirection de ports. Le schéma ci dessous résume le principe du port forwarding.

![Untitled](Connection%20distante%20A%20%E2%86%90%E2%86%92B%20f7beb932d2824b68a45e93060c2472cd/Untitled%201.png)

Le schéma ci dessus est décrit par la commande suivante :

```bash
ssh -L localhost:9099:tp-hadoop-29:4040 bridge_name@bridge_ip_adresse
```

Après avoir exécuter cette commande, le terminal se connecte au bridge. Il faut bien garder ce terminal ouvert car c’est lui qui sert de tunnel. Si vous le fermer, votre connexion est aussi tôt interrompue. 

Ouvrez un nouveau terminal et vérifier que le port 9099 local est bien en mode LISTEN; vous pouvez utiliser la commande ci dessous : 

```bash
lsof -iTCP -sTCP:LISTEN -n -P
```

![Untitled](Connection%20distante%20A%20%E2%86%90%E2%86%92B%20f7beb932d2824b68a45e93060c2472cd/Untitled%202.png)

A présent on peut démarrer le navigateur et accéder au service Spark souhaité

![Untitled](Connection%20distante%20A%20%E2%86%90%E2%86%92B%20f7beb932d2824b68a45e93060c2472cd/Untitled%203.png)

## Bonus

```bash
nslookup tp-hadoop-29 # to see the ip adress hidden behind a custom name
```