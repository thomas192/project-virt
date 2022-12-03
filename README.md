# Projet Virtualisation

Thomas Saudemont - Yoann Hamel-Muller


## Description


Les conteneurs ```command_post```et ```radio_op1``` sont situ�s dans le m�me docker-compose sur une premi�re machine (VM1). ```command_post``` envoie un ordre � ```radio_op1```. Ce dernier relaie ensuite le message � ```radio_op2``` situ� sur une deuxi�me machine (VM2) qui transmet enfin le message � ```officer```. Tout comme pour la premi�re machine ces deux conteneurs sont voisins au sein du m�me docker-compose). ```officer``` se charge d'ex�cuter les ordres.


```command_post``` envoie deux types d'ordres:

- spawn: g�n�re une arm�e de processus zombie

- kill: tue le g�n�rateur (processus parent)


Tuer le processus parent est suppos� tuer les zombies (enfants) par la m�me occasion. Or, il se trouve que ce n'est pas le cas ici contrairement � ce qui a �t� constat� dans la voie de la fourchette (voir logs du conteneur ```officer```). Les programmes C utilis�s ici pour g�rer les zombies sont des versions abr�g�es du programme de la voie de la fourchette. Se r�f�rer � la version compl�te pour la d�monstration (```zombies.c```).


## Utilisation


Vis-�-vis de Proxmox, la VM2 (receveur) correspond � ```thomas```, et la VM1 (envoyeur) � ```noe```.  L'adresse IP de la VM2 se sp�cifie dans le fichier ```relay_to_radio_op2.py``` (```DEST_HOST=<IP>```) situ� dans ```VM1/radio_op1```. Cela est d�j� fait. Les adresses IP ne devraient pas changer.


Dans l'ordre, lancer sur la VM2 les conteneurs:


- officer: ```sudo docker-compose build officer && sudo docker-compose up officer```

- radio_op2: ```sudo docker-compose build radio_op2 && sudo docker-compose up radio_op2```


Puis sur la VM1 les conteneurs:


- radio_op1: ```sudo docker-compose build radio_op1 && sudo docker-compose up radio_op1```

- command_post: ```sudo docker-compose build command_post && sudo docker-compose up command_post```


Chaque conteneur affiche des logs � toutes les �tapes de la cha�ne de commande. Il est possible de relancer le conteneur ```command_post``` plusieurs fois.


Une version du projet qui fonctionne dans un seul et m�me docker-compose est jointe (```command_chain_local```). Il s'utilise de la m�me mani�re.