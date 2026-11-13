Agence de voyage – Gestion des vols et des réservations
Application console en C++ (programmation orientée objet) qui informatise la gestion d'une agence de voyage : passagers, vols, réservations de billets d'avion, paiements et annulations.
Le projet comprend une partie modélisation UML (diagramme de classes) et une partie implémentation en C++.
Fichier
Description
agence_voyage.cpp
Code source complet de l'application (un seul fichier)
diagramme_classes.puml
Code PlantUML du diagramme de classes
diagramme_classes.jpg
Image du diagramme de classes UML
README.md
Ce document
2. Fonctionnalités
Enregistrer des passagers (nom, prénom, téléphone, email).
Ajouter des vols (numéro, départ, destination, date, capacité).
Afficher les vols disponibles (avec places libres et pas encore partis).
Rechercher un vol par numéro, ville de départ ou destination.
Effectuer une réservation pour un passager sur un vol.
Enregistrer un paiement, ce qui valide la réservation.
Annuler une réservation, avec gestion du remboursement.
Consulter une réservation (résumé) et afficher toutes ses informations (détails).
Code::Blocks : Settings → Compiler → Compiler flags, cocher l'option C++17 (ou C++14), puis Build and run.
Dev-C++ : Tools → Compiler Options → Settings → Code Generation, choisir ISO C++17 (ou C++14) dans Language standard.
Visual Studio : Propriétés du projet → C/C++ → Langage → Norme du langage C++, choisir C++17.
Exécution
./agence          # Linux / macOS
agence.exe        # Windows
Accents sous Windows
Le programme active automatiquement l'encodage UTF-8 de la console. Si les accents s'affichent mal, enregistre le fichier source en UTF-8 ou tape chcp 65001 dans l'invite de commandes avant de lancer l'application.
5. Exécution et menu principal
========== AGENCE DE VOYAGE ==========
 1. Enregistrer un passager
 2. Ajouter un vol
 3. Afficher les vols disponibles
 4. Rechercher un vol
 5. Effectuer une réservation
 6. Enregistrer un paiement
 7. Annuler une réservation
 8. Consulter une réservation
 9. Afficher les informations d'une réservation
10. Quitter
Option
Action
1
Saisie du nom, prénom, téléphone, email. Un numéro de passager est attribué automatiquement.
2
Saisie du numéro de vol, de la ville de départ, de la destination, de la date (JJ/MM/AAAA) et de la capacité.
3
Liste des vols qui ont encore des places et qui ne sont pas partis.
4
Recherche par numéro de vol, ville de départ ou destination (sans tenir compte des majuscules).
5
Crée une réservation (statut EN_ATTENTE) pour un passager sur un vol.
6
Enregistre le paiement d'une réservation et la valide (statut VALIDEE).
7
Annule une réservation et applique la règle de remboursement.
8
Affiche le résumé d'une réservation sur une ligne.
9
Affiche les détails complets : passager, vol, paiement, statut de remboursement.
10
Quitte l'application.
6. Exemple d'utilisation
Exemple de session (les dates varient selon le jour d'exécution).
Votre choix : 1
--- Enregistrer un passager ---
Nom : Diallo
Prénom : Aminata
Téléphone : 770000001
Email : aminata@mail.com
Passager enregistré avec le n°1.

Votre choix : 2
--- Ajouter un vol ---
Numéro du vol : AF101
Ville de départ : Paris
Destination : Dakar
Date de départ (JJ/MM/AAAA) : 15/06/2030
Capacité (nombre de places) : 2
Vol ajouté.

Votre choix : 5
--- Effectuer une réservation ---
N° du passager : 1
Numéro du vol : AF101
Réservation créée. Elle sera validée à l'enregistrement du paiement.
Réservation n°1 | EN_ATTENTE | Aminata Diallo | Vol AF101 | Paiement : aucun

Votre choix : 6
--- Enregistrer un paiement ---
N° de la réservation : 1
Montant : 100
Paiement enregistré : la réservation est validée, la place est réservée.

Votre choix : 9
--- Informations d'une réservation ---
N° de la réservation : 1
Réservation n°1 | Date : 13/11/2026 | Statut : VALIDEE
   Passager : Aminata Diallo (n°1) | Tél : 770000001 | Email : aminata@mail.com
   Vol      : AF101 | Paris -> Dakar | Départ : 15/06/2030
   Paiement : n°1 | Montant : 100.00 | Date : 13/11/2026 | Statut : PAYE

Votre choix : 7
--- Annuler une réservation ---
N° de la réservation à annuler : 1
Réservation annulée.
Réservation n°1 | Date : 13/11/2026 | Statut : ANNULEE
   ...
   Paiement : n°1 | Montant : 100.00 | Date : 13/11/2026 | Statut : REMBOURSE
7. Modélisation UML
Le diagramme de classes est fourni dans diagramme_classes.jpg (code PlantUML dans diagramme_classes.puml).
Classes
Classe
Rôle
Passager
Personne qui voyage : id, nom, prénom, téléphone, email
Vol
Trajet proposé : numéro, départ, destination, date, capacité, places réservées
Reservation
Lien entre un passager et un vol : id, date, statut
Paiement
Règlement d'une réservation : id, montant, date, statut
Agence
Classe de gestion : regroupe les passagers, vols et réservations
StatutReservation
Énumération : EN_ATTENTE, VALIDEE, ANNULEE
StatutPaiement
Énumération : PAYE, REMBOURSE, NON_REMBOURSABLE
Associations et multiplicités
Association
Multiplicités
Passager – Reservation
1 passager – 0..* réservations
Vol – Reservation
1 vol – 0..* réservations
Reservation – Paiement
1 réservation – 0..1 paiement
Agence – Passager / Vol / Reservation
1 agence – 0..* de chacun (agrégation)
8. Règles de gestion
N°
Règle
Où elle est appliquée
1
Un passager doit être enregistré avant de réserver.
Agence::reserver()
2
Un vol ne peut pas dépasser sa capacité.
Vol::reserverPlace() et Agence::reserver()
3
Un passager ne peut pas réserver deux fois le même vol.
Agence::reserver() (réservations non annulées)
4
Une réservation est liée à un seul passager et à un seul vol.
Structure de Reservation
5
Une réservation annulée ne peut plus recevoir de paiement.
Reservation::enregistrerPaiement()
6
Une place n'est définitivement réservée qu'après validation.
Reservation::valider(), appelée lors du paiement
7
Une réservation payée puis annulée suit le statut de remboursement prévu.
Agence::annuler()
9. Conception et principes de POO
Encapsulation : tous les attributs sont privés, avec des accesseurs (get…) et des modificateurs (set…) qui contrôlent la validité des données.
Constructeurs : chaque classe a un constructeur qui initialise et vérifie ses données (par exemple, un Paiement refuse un montant négatif).
Méthodes adaptées : reserverPlace(), libererPlace(), valider(), annuler(), effectuer(), rembourser(), etc.
Associations entre objets :
une Reservation référence un Passager et un Vol par pointeurs ;
elle possède son Paiement (composition, unique_ptr) ;
l'Agence possède toutes les données.
Collections vector : l'Agence stocke les passagers, vols et réservations dans des vector<unique_ptr<…>>, ce qui garde des adresses stables pour les pointeurs des réservations.
Énumérations (enum class) pour les statuts.
Séparation entre la logique métier (classes) et l'interface console (fonctions de saisie et de menu).
10. Gestion des erreurs
Erreurs de saisie
La question est reposée tant que la valeur est invalide :
texte saisi à la place d'un nombre ;
nom, prénom, numéro de vol, ville ou destination vides ;
téléphone invalide (8 à 15 chiffres, + et espaces autorisés) ;
email invalide (forme x@y.z attendue) ;
date inexistante ou mal formatée (par exemple 31/02/2030) ;
capacité ou montant nul ou négatif ;
choix de menu hors de la plage 1 à 10.
Opérations interdites
Un message d'erreur est affiché et rien n'est modifié :
réserver pour un passager non enregistré ;
réserver un vol inexistant, déjà parti ou complet ;
réserver deux fois le même vol ;
ajouter un vol dont le numéro existe déjà ou dont la date est passée ;
payer une réservation annulée ou déjà payée ;
payer quand le vol est devenu complet entre-temps ;
annuler une réservation déjà annulée ;
consulter une réservation introuvable.
Ces erreurs sont signalées par des exceptions (invalid_argument, runtime_error) attrapées dans la boucle du menu principal.
11. Hypothèses et limites
Validation par le paiement : le menu ne propose pas d'option « Valider ». C'est l'enregistrement du paiement qui valide la réservation et réserve la place.
Places réservées : seules les réservations validées occupent une place. Plusieurs réservations en attente peuvent donc viser la dernière place ; la première payée l'obtient.
Remboursement (règle 7) : le sujet ne le précise pas. Hypothèse retenue : REMBOURSE si l'annulation a lieu plus de 24 h avant le départ, sinon NON_REMBOURSABLE. Le départ est compté à minuit le jour de la date du vol.
Réservation après annulation : un passager peut réserver de nouveau un vol dont il a annulé la réservation.
Identifiants : les numéros de passager, de réservation et de paiement sont attribués automatiquement.
Pas de sauvegarde : les données sont en mémoire et disparaissent à la fermeture du programme.
Menu : les méthodes de modification et de suppression d'un passager ou d'un vol existent dans la classe Agence (conformément au diagramme), mais ne sont pas appelées par le menu imposé.
Montants : le programme n'a pas de tarif par vol ; le montant est saisi librement lors du paiement.
12. Tests effectués
Un scénario complet a été exécuté avec succès :
saisies invalides (téléphone, email, date, capacité, montant, choix de menu) rejetées puis redemandées ;
ajout d'un vol existant ou dont la date est passée, refusé ;
réservation pour un passager inconnu, refusée ;
doublon passager + vol, refusé ;
vol complet : paiement de la deuxième réservation refusé, puis accepté après l'annulation de la première ;
paiement sur une réservation annulée, refusé ;
annulation avec remboursement (départ lointain) : statut REMBOURSE ;
annulation avec départ le lendemain : statut NON_REMBOURSABLE ;
double annulation et consultation d'une réservation inexistante, refusées.
Auteur
Projet réalisé dans le cadre d'un TP de modélisation UML et de programmation orientée objet en C++.
