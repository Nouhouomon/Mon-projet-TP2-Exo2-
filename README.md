# Mon-projet-TP2-Exo2-
#le code pour l'implémentation en C++(Travaux pratiques N°2)







#Le code pour obtenir le diagramme en UML(exercices 2)
@startuml
skinparam classAttributeIconSize 0

class Filiere {
  - code : string
  - nom : string
  + ajouterUE(ue : UE*) : void
  + getUEs() : vector<UE*>
}

class UE {
  - code : string
  - intitule : string
  - credit : int
  + ajouterMatiere(m : Matiere*) : void
  + calculerMoyenne(e : Etudiant) : double
  + estValidee(e : Etudiant) : bool
}

class Matiere {
  - code : string
  - intitule : string
  - coefficient : double
  + getCoefficient() : double
}

class Etudiant {
  - matricule : string
  - nom : string
  - prenom : string
  - niveau : string
  + ajouterNote(n : Note*) : void
  + calculerMoyenne() : double
  + afficherReleve() : void
}

class Note {
  - valeur : double
  + getValeur() : double
}

Filiere "1" o-- "1..*" UE : comporte
UE "1" *-- "1..*" Matiere : est composée de
Filiere "1" o-- "0..*" Etudiant : regroupe
Etudiant "1" *-- "0..*" Note : obtient
Note "0..*" --> "1" Matiere : concerne

@enduml
