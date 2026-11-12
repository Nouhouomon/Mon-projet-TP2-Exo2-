Projet : Système de gestion des résultats des étudiants d'un département universitaire
Réalisé par NOUHOUMON K Lucien Sèjlo, étudiant en licence 3 de Mathématiques et Informatique Appliquée à l'UNSTIM Fast-Nati.
Présentation du projet
Ce projet consiste à concevoir et à implémenter en C++ un système de gestion des résultats des étudiants d'un département universitaire. Il répond au besoin du département de disposer d'un outil fiable pour enregistrer les notes, calculer les moyennes, vérifier la validation des unités d'enseignement et produire les relevés de notes.
Contexte et règles de gestion
Un étudiant appartient à une filière et à un niveau d'étude (L1, L2 ou L3). Chaque filière comporte plusieurs unités d'enseignement (UE). Chaque UE possède un nombre de crédits et se compose d'une ou plusieurs matières. Les étudiants obtiennent des notes dans les différentes matières.
Analyse et conception
L'analyse du sujet fait apparaître cinq classes : Etudiant, Filiere, UE, Matiere et Note. La classe Etudiant décrit l'étudiant (identité, niveau, filière) et conserve l'ensemble de ses notes. La classe Filiere regroupe les unités d'enseignement qui la composent. La classe UE porte un intitulé et un nombre de crédits, et regroupe ses matières. La classe Matiere représente une discipline, avec son intitulé et son coefficient. La classe Note associe une valeur à un étudiant et à une matière.
Relations entre les classes
Une filière est composée de plusieurs UE (multiplicité 1 à 1..), et une UE est composée d'une ou plusieurs matières (1 à 1..). Ces deux liens sont des compositions, car une UE n'existe pas sans sa filière ni une matière sans son UE. Un étudiant est rattaché à une seule filière, alors qu'une filière accueille plusieurs étudiants (1 à 0..), ce qui correspond à une agrégation, l'étudiant pouvant exister indépendamment de la filière. Enfin, un étudiant possède plusieurs notes (1 à 0..), et chaque note concerne une seule matière (0..* à 1).
Fonctionnalités à implémenter
Le programme permet de calculer la moyenne générale d'un étudiant à partir de ses notes et des coefficients des matières, de déterminer si un étudiant valide une UE selon sa moyenne dans cette UE, et d'afficher le relevé de notes d'un étudiant (matières, notes, moyennes par UE, moyenne générale et décision).
Notions mobilisées
Le projet met en pratique l'encapsulation (attributs privés, accesseurs et mutateurs), la composition et l'agrégation, les relations entre objets, l'utilisation de tableaux et de collections d'objets (vector), les méthodes de calcul, ainsi que la notation UML des multiplicités dans le diagramme de classes.
Livrables
Le projet comprend le diagramme de classes UML, le code source C++ des cinq classes avec un programme principal de démonstration, et l'affichage du relevé de notes d'un étudiant.
