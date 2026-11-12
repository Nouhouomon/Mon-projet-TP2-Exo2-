% Mon-projet-TP2-Exo2-
%le code pour l'implémentation en C++(Travaux pratiques N°2)

#include <iostream>
#include <string>
#include <vector>
#include <iomanip>
#include <stdexcept>
using namespace std;

// ---------- Matiere ----------
class Matiere {
    string code, intitule;
    double coefficient;
public:
    Matiere(string c, string i, double coef)
        : code(c), intitule(i), coefficient(coef) {
        if (coef <= 0) throw invalid_argument("Coefficient invalide");
    }
    string getCode() const { return code; }
    string getIntitule() const { return intitule; }
    double getCoefficient() const { return coefficient; }
};

// ---------- UE (composition de Matiere) ----------
class UE {
    string code, intitule;
    int credit;
    vector<Matiere> matieres;           // composition
public:
    UE(string c, string i, int cr) : code(c), intitule(i), credit(cr) {}
    void ajouterMatiere(const Matiere& m) { matieres.push_back(m); }
    string getCode() const { return code; }
    string getIntitule() const { return intitule; }
    int getCredit() const { return credit; }
    const vector<Matiere>& getMatieres() const { return matieres; }
};

// ---------- Filiere (composition d'UE) ----------
class Filiere {
    string code, nom;
    vector<UE> ues;                     // composition
public:
    Filiere(string c, string n) : code(c), nom(n) {}
    void ajouterUE(const UE& ue) { ues.push_back(ue); }
    string getNom() const { return nom; }
    const vector<UE>& getUEs() const { return ues; }
};

// ---------- Note (classe d'association) ----------
class Note {
    const Matiere* matiere;             // association
    double valeur;
public:
    Note(const Matiere* m, double v) : matiere(m), valeur(v) {
        if (v < 0 || v > 20) throw invalid_argument("Note hors [0;20]");
    }
    const Matiere* getMatiere() const { return matiere; }
    double getValeur() const { return valeur; }
};

// ---------- Etudiant ----------
class Etudiant {
    string matricule, nom, prenom, niveau;
    const Filiere* filiere;             // agrégation
    vector<Note> notes;                 // composition
public:
    Etudiant(string mat, string n, string p, string niv, const Filiere* f)
        : matricule(mat), nom(n), prenom(p), niveau(niv), filiere(f) {}

    void ajouterNote(const Matiere& m, double valeur) {
        notes.emplace_back(&m, valeur);
    }

    // Note dans une matière (0 si non encore saisie)
    double noteDe(const Matiere& m) const {
        for (const Note& n : notes)
            if (n.getMatiere()->getCode() == m.getCode())
                return n.getValeur();
        return 0.0;
    }

    double moyenneUE(const UE& ue) const {
        double somme = 0, coefs = 0;
        for (const Matiere& m : ue.getMatieres()) {
            somme += noteDe(m) * m.getCoefficient();
            coefs += m.getCoefficient();
        }
        return coefs > 0 ? somme / coefs : 0.0;
    }

    bool valideUE(const UE& ue) const { return moyenneUE(ue) >= 10.0; }

    double moyenneGenerale() const {
        double somme = 0; int credits = 0;
        for (const UE& ue : filiere->getUEs()) {
            somme += moyenneUE(ue) * ue.getCredit();
            credits += ue.getCredit();
        }
        return credits > 0 ? somme / credits : 0.0;
    }

    void afficherReleve() const {
        cout << fixed << setprecision(2);
        cout << "===== RELEVE DE NOTES =====\n"
             << nom << " " << prenom << " (" << matricule << ")\n"
             << "Filiere : " << filiere->getNom() << " - " << niveau << "\n\n";
        for (const UE& ue : filiere->getUEs()) {
            cout << ue.getIntitule() << " [" << ue.getCredit() << " crédits]\n";
            for (const Matiere& m : ue.getMatieres())
                cout << "   " << setw(22) << left << m.getIntitule()
                     << " coef " << m.getCoefficient()
                     << "  note : " << noteDe(m) << "\n";
            cout << "   Moyenne UE : " << moyenneUE(ue) << "  -> "
                 << (valideUE(ue) ? "VALIDEE" : "NON VALIDEE") << "\n\n";
        }
        cout << "Moyenne générale : " << moyenneGenerale() << "/20\n";
    }
};

// ---------- main ----------
int main() {
    UE algo("UE1", "Algorithmique", 6);
    algo.ajouterMatiere(Matiere("M1", "Programmation C++", 3));
    algo.ajouterMatiere(Matiere("M2", "Structures de données", 2));

    UE maths("UE2", "Mathématiques", 4);
    maths.ajouterMatiere(Matiere("M3", "Analyse", 2));
    maths.ajouterMatiere(Matiere("M4", "Algèbre", 2));

    Filiere info("MI", "Mathématiques-Informatique");
    info.ajouterUE(algo);      // la filière doit être complète AVANT
    info.ajouterUE(maths);     // de créer les notes (pointeurs stables)

    Etudiant e("E001", "Doe", "John", "L3", &info);
    const auto& ues = info.getUEs();
    e.ajouterNote(ues[0].getMatieres()[0], 14);
    e.ajouterNote(ues[0].getMatieres()[1], 11);
    e.ajouterNote(ues[1].getMatieres()[0], 8);
    e.ajouterNote(ues[1].getMatieres()[1], 10);

    e.afficherReleve();
    return 0;
}


















