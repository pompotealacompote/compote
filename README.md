# SonarQube - Intégration de GitHub et Jenkins

## Introduction
Bienvenue dans la présentation du projet SonarQube ! Cette documentation vous guidera à travers les étapes nécessaires pour comprendre comment fonctionnent GitHub, Jenkins et SonarQube ensemble. Ces outils sont largement utilisés dans le développement logiciel pour faciliter la collaboration, l'intégration continue et l'analyse de la qualité du code.

## Table des matières

- [1. Qu'est-ce que GitHub ?](#1-Qu-est-ce-que-GitHub-)
- [2. Qu'est-ce que Jenkins ?](#2-qu-est-ce-que-jenkins-)
- [3. Qu'est-ce que SonarQube ?](#3-qu-est-ce-que-sonarqube-)
- [4. Comment fonctionnent GitHub, Jenkins et SonarQube ensemble ?](#4-comment-fonctionnent-github-jenkins-et-sonarqube-ensemble-)
- [5. Installation des plugins sur Jekins](#5-installation-des-plugins-sur-jenkins)
- [6. Configuration de la liaion entre SonarQube et Jenkins](#6-configuration-de-la-liaion-entre-sonarqube-et-jenkins)
- [7. Configuration de l'intégration entre GitHub et Jenkins](#7-configuration-de-l-intégration-entre-gitHub-et-jenkins)
- [8. Option supplémentaire avec Jenkins](#8-option-supplémentaire-avec-jenkins)
- [9. Conclusion](#9-conclusion)

## 1. Qu est ce que GitHub ?
GitHub est une plateforme de développement collaboratif basée sur Git. Elle permet aux développeurs de stocker, gérer et partager leur code source, ainsi que de collaborer efficacement avec d'autres contributeurs. GitHub fournit également des fonctionnalités telles que le suivi des problèmes, les demandes d'extraction (pull requests) et l'hébergement gratuit de dépôts de code open source.

## 2. Qu est ce que Jenkins ?
Jenkins est un outil open source d'intégration continue, essentiel dans les projets de développement logiciel. Il automatise le processus de construction, de test et de déploiement des applications, ce qui permet de détecter rapidement les erreurs et de faciliter la collaboration entre les membres de l'équipe. Jenkins peut être configuré pour s'intégrer avec différents outils de gestion de code source tels que GitHub.

## 3. Qu est-ce que SonarQube ?
SonarQube est une plateforme d'analyse de la qualité du code. Elle fournit des informations détaillées sur la qualité du code source en détectant les problèmes potentiels, les vulnérabilités de sécurité et les violations des bonnes pratiques de développement. SonarQube prend en charge plusieurs langages de programmation et fournit des métriques et des rapports pour aider les équipes à améliorer la qualité de leur code.

## 4. Comment fonctionnent GitHub, Jenkins et SonarQube ensemble ?

![alt text](https://github.com/mi5hell/WebSite/blob/main/.src/schema-infra.jpg)

L'intégration de GitHub, Jenkins et SonarQube permet de mettre en place un flux de travail cohérent pour le développement logiciel. Voici comment ils interagissent ensemble :

- L'équipe de développement utilise GitHub pour héberger le code source de leur projet. Ils créent des branches, effectuent des modifications et proposent des demandes d'extraction pour intégrer les nouvelles fonctionnalités ou les corrections de bugs.

- Jenkins est configuré pour surveiller les modifications du code source sur GitHub. Chaque fois qu'une modification est détectée, Jenkins déclenche automatiquement une série d'actions définies dans un pipeline. Ces actions peuvent inclure la compilation du code, l'exécution des tests unitaires et le déploiement de l'application.

- À l'étape d'analyse du code, Jenkins peut se connecter à SonarQube pour obtenir des rapports détaillés sur la qualité du code. SonarQube analyse le code source à la recherche de problèmes de qualité tels que des bugs, des vulnérabilités ou des violations des bonnes pratiques de développement.

- Les résultats de l'analyse de SonarQube peuvent être intégrés dans le pipeline de Jenkins. Par exemple, si la qualité du code ne répond pas aux critères définis, Jenkins peut signaler une erreur ou empêcher le déploiement de l'application.

## 5. Installation des plugins sur Jenkins
Deux plugins sont nécessaires pour permettre la communication entres les différentes solutions :
1. Administraion > Gestion des plugins :
			- Maven Integration
			- SonarQube Scanner
				  - Configuration à faire > Configuration globale des outils > Ajouter SonarQube Scanner

## 6. Configuration de la liaion entre SonarQube et Jenkins
Pour configurer la liaion entre SonarQube et Jenkins, suivez les étapes suivantes :

1. Première étape sur Sonarqube :
  - Administration > Security > Générer un token pour Jenkins
2. Deuxième étape sur Jenkins :
  - Administration > Credentials > System > Identifiants globaux > Add Credentials :
      - Ajouter le token généré dans Sonarqube, cela va permettre la communication entre les deux solutions
  - Administration > Configure System > SonarQube Servers :
      - Cocher Environment variables
	    - Ajouter une installation SonarQube :
		      - URL du serveur SonarQube
		      - Insérer le token ajouter précédent

## 7. Configuration de l intégration entre GitHub et Jenkins
Pour configurer l'intégration entre GitHub et Jenkins, suivez les étapes suivantes :

1. New Item > Projet Free-Style :
    - Github Project + Code source GIT (définir la branche)
		- Build Steps > Lancer une analyse avec SonarQube Scanner :
		   - Ici on définit l'analyse avec le nom du projet, le token de sonarqube et ce qu'on souhaite analyser sur github (notamment le format des fichiers).
		- Lancer le build
2. Retourner sur Sonarqube pour vérifier le traitement des données

## 8. Option supplémentaire avec Jenkins
Il est possible d'ajouter des fonctionnalités sur Jenkins, notamment d'automatisation du lancement du Build et également pour prendre en compte les vulnérabilités présentes sur la partie HTML avec OWASP. 

Le Top 10 OWASP (Open Web Application Security Project), il s'agit d'une liste des dix principales vulnérabilités de sécurité des applications web. Cette liste est établie par une organisation à but non lucratif qui vise à améliorer la sécurité des logiciels. Les dix principales vulnérabilités OWASP fournissent un guide précieux pour les développeurs afin de se prémunir contre les failles les plus courantes et de renforcer la sécurité de leurs applications.Il est possible de l'intégrer via un plugins dans jenkins.

Pour cela, suivez ces étapes :

1. Automisation du lancement du Build sur Jenkins :
	  - Ajouter le Plugin : Parameterized Scheduler
	  - Puis on retourne sur le projet dans jenkins > Configurer > Ce qui déclenche le build > Construire périodiquement > Insérer le temps que l'on souhaite
		    - Nous avons mis toutes les 10 minutes avec : H/10 * * * *

2. Prendre en compte les vulnérabilités HTML ( OAWSP ) :
	  - Ajouter le Plugin : OWASP Markup Formatter Plugin
	  - Dans Administrer Jenkins > Configurer la sécurité globale > Markup Formatter > Safe HTML
		    - Puis sur SonarQube > sur notre projet > Issues :
		        -> Un nouvel onglet Security Category est présent ou l'on peut voir les vulnérabilitées 

## 9. Conclusion
GitHub, Jenkins et SonarQube sont des outils puissants qui peuvent grandement améliorer le processus de développement logiciel. L'intégration de ces outils permet une collaboration efficace, une intégration continue et une analyse approfondie de la qualité du code. En utilisant GitHub pour gérer le code source, Jenkins pour l'intégration continue et SonarQube pour l'analyse de la qualité, les équipes de développement peuvent travailler plus efficacement et produire un code de meilleure qualité. N'hésitez pas à explorer davantage ces outils et à les intégrer à vos propres projets !
