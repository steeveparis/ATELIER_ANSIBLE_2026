------------------------------------------------------------------------------------------------------
ATELIER ANSIBLE
------------------------------------------------------------------------------------------------------
L’idée en 30 secondes : Dans cet atelier, vous allez apprendre à **automatiser le déploiement d’un serveur web avec Ansible**, directement depuis GitHub **Codespaces**, sans infrastructure complexe. L’objectif est de comprendre comment **décrire un état cible** (installer, configurer, déployer) et laisser l’outil l’appliquer automatiquement, de manière reproductible et idempotente. On passe ainsi d’une logique manuelle à une logique DevOps industrialisée.
  
**Architecture cible :** Ci-dessous, voici l'architecture cible souhaitée.   
  
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/8064eb95-da73-4bdd-9ef2-a7cebbbd71c8" />
  
-------------------------------------------------------------------------------------------------------
Séquence 1 : Codespace de Github
-------------------------------------------------------------------------------------------------------
Objectif : Création d'un Codespace Github  
Difficulté : Très facile (~5 minutes)
-------------------------------------------------------------------------------------------------------
**Faites un Fork de ce projet**. Si besoin, voici une vidéo d'accompagnement pour vous aider à "Forker" un Repository Github : [Forker ce projet](https://youtu.be/p33-7XQ29zQ) 
  
Ensuite depuis l'onglet **[CODE]** de votre nouveau Repository, **ouvrez un Codespace Github**.
  
---------------------------------------------------
Séquence 2 : Création du votre environnement de travail
---------------------------------------------------
Objectif : Créer votre environnement de travail  
Difficulté : Simple (~10 minutes)
---------------------------------------------------
Vous allez dans cette séquence mettre en place votre environnement et les logiciels Ansible. Depuis le terminal de votre Codespace copier/coller les codes ci-dessous étape par étape :  

**Installation de Ansible et Nginx**  
```
sudo apt update
sudo apt install -y ansible curl
```
**Vérifier l’environnement**  
```
ansible --version
curl --version
```
**Tester la cible locale**  
```
ansible -i inventory.ini local -m ping
```
**Exécuter le playbook**  
```
ansible-playbook -i inventory.ini playbook.yml
```
**Vérifier le résultat**  
```
curl http://localhost
```  
---------------------------------------------------  
**Réccupération de l'URL de votre serveur Nginx**. Votre serveur Nginx (et sa page Web) est déployé dans Codespace. Pour obtenir votre URL cliquez sur l'onglet **[PORTS]** dans votre Codespace (à coté de Terminal), ouvrez le port 80 et rendez public votre port (Visibilité du port). Ouvrez l'URL dans votre navigateur et c'est terminé.  
  
---------------------------------------------------
Séquence 3 : Exercices  
Difficulté : Facile (~30 minutes)
---------------------------------------------------
### Exercice 1 : Customisation de la page d'accueil 
Customisez la page d'accueil de votre site Web en ajoutant la ligne suivante dans votre fichier index.html  
```
<h1>Déploiement réalisé par [Votre Nom]</h1>
```

### Exercice 2 : Paramétrer dynamiquement votre serveur avec Ansible 
Voici le contenu (contenu imposé) de votre nouveau fichier index.html    
```
<!DOCTYPE html>
<html>
<head>
  <title>{{ page_title }}</title>
</head>
<body>
  <h1>{{ page_title }}</h1>
  <p>Déployé par : {{ author }}</p>
  <p>Utilisateur système : {{ app_user }}</p>
</body>
</html>
```
Modifier votre playbook afin de :  
* Créer un title contenant le texte suivant : "Serveur déployé avec Ansible"
* L'auteur sera : "Votre nom"
* L'utilisateur sera un **utilisateur Linux** : "Votre prénom"
  
---------------------------------------------------
Séquence 4 : Questions  
Difficulté : Moyenne (~45 minutes)
---------------------------------------------------
**Complétez et documentez ce fichier README.md** pour répondre aux questions des exercices.  
Faites preuve de pédagogie et soyez clair dans vos explications et procedures de travail.  

**Question 1 :**  
Pourquoi Ansible est-il qualifié d’outil "déclaratif" ?    
  
Ansible est dit déclaratif parce qu’on ne donne pas une suite de commandes à exécuter, mais plutôt le résultat qu’on veut obtenir.
Par exemple, on dit que Nginx doit être installé et démarré, et Ansible se débrouille pour que ce soit le cas.

Du coup, on ne se concentre pas sur “comment faire”, mais sur “ce qu’on veut à la fin”.
Ça rend les déploiements plus simples et surtout répétables sans erreur.

**Question 2 :**  
Pourquoi l’utilisation de variables est-elle essentielle dans un playbook ?  
  
Les variables permettent de ne pas écrire des valeurs en dur partout.
Par exemple, au lieu d’écrire directement le titre ou le nom de l’auteur dans le HTML, on utilise des variables comme page_title ou author.

**Question 3 :**  
En quoi Ansible facilite-t-il la gestion de plusieurs serveurs ?  
  
Avec Ansible, on peut appliquer le même playbook à plusieurs machines en même temps.

Au lieu de configurer chaque serveur à la main, on écrit une seule fois les règles, et Ansible les applique partout via l’inventaire.

**Question 4 :**  
Quels sont les avantages et les limites d’Ansible dans un contexte DevOps ?   
  
Avantages :
automatisation des tâches
déploiements reproductibles
facile à lire et à comprendre
pas besoin d’agent sur les machines
Limites :
moins adapté pour créer l’infrastructure (là on utilise Terraform)
peut devenir compliqué sur de très gros projets si mal organisé

En pratique, on utilise souvent Terraform + Ansible :

Terraform → crée les machines
Ansible → configure les machines
  
**Question 5 :**  
Quelle est la différence entre les modules copy et template dans Ansible ?   
  
copy : copie un fichier tel quel
template : copie un fichier en remplaçant les variables

Dans ton cas :

copy → ne marche pas avec {{ page_title }}
template → remplace les variables automatiquement

Donc dès qu’il y a des variables → on utilise template

---------------------------------------------------
Séquence 5 : Atelier  
Difficulté : Moyenne (~1 heure)
---------------------------------------------------
### Structurer votre déploiement Ansible afin de pouvoir choisir entre un rôle DEV ou un rôle PROD  
Modifier votre fichier playbook.yml afin de pouvoir choisir entre un rôle DEV ou un rôle PROD.  

**Resultats attendus**  
* Déploiement en DEV  
```
ansible-playbook -i inventory.ini playbook.yml --limit dev
```
```
app_name: "Application DEV"
env: "dev"
author: "Etudiant DEV"
```
* Déploiement en PROD
```
ansible-playbook -i inventory.ini playbook.yml --limit prod
```
```
app_name: "Application PROD"
env: "prod"
author: "Equipe Ops""
```
---------------------------------------------------
### Zoom sur votre environnement Codepace 
Codepace est un outil proposer par GitHub soumit à quota (4$/mois). Si vous dépasser votre quota mensuel vous ne serez plus en mesure de pouvoir utiliser Codespace. C'est pourquoi à **la fin de votre atelier, pensez à suprimer votre Codespace après avoir mis à jour votre GitHub**.  

### Vous pouvez voir l'état de vos Codespace ici : https://github.com/codespaces  
  
---------------------------------------------------
Evaluation
---------------------------------------------------
Cet atelier ANSIBLE, **noté sur 20 points**, est évalué sur la base du barème suivant :  
- Mise en oeuvre (2 points)
- Exercice N°1 - Customisation de la page d'accueil (2 points)
- Exercice N°2 - Paramétrer dynamiquement votre serveur avec Ansible (2 points)
- Questions + Qualité du Readme (lisibilité, erreur, ...) (5 points)
- Atelier - Rôles DEV et PROD (6 points)
- Processus travail (quantité de commits, cohérence globale, interventions externes, ...) (3 points) 
