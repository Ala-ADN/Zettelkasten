$$Ala\ eddine\ Zaouali$$
$$Aya\ Belkacem$$
$$Kacem\ Mathlouthi$$
# Exercise 1
### Exécution des requêtes
```Prolog
?- habite_la_meme_ville(X, g).
X = g ;
X = j ;
X = 1.

?- habite_a(X, tunis).
X = k ;
X = e ;
X = m ;
X = n.

?- mineur(X), habite_a(X, tunis).
X = m ;
X = n.

?- epoux_possible(X, g).
X = e ;
X = i;
flase.

?- epoux_possible(X, Y), habite_a(X, tunis).
X = e, Y = g ;
X = e, Y = h ;
X = e, Y = m;
flase.


?- mineur(X).
X = j ;
X = 1 ;
X = m ;
X = n.

?- meme_age(X, Y).
X = Y ;
X = k, Y = k ;
X = c, Y = c ;
X = d, Y = d ;
X = e, Y = e ;
X = g, Y = g ;
X = h, Y = h ;
X = i, Y = i ;
X = j, Y = j ;
X = 1, Y = 1 ;
X = m, Y = m ;
X = n, Y = n.
```

# Exercise 2
```prolog
[trace] 2 ?- bonne_humeur(X). 
   % Recherche des valeurs possibles de X.
     Call: (22) bonne_humeur(_4675) ? creep % Vérification de bonne_humeur(X).
     Call: (23) argent(_4675) ? creep % Vérification si X a de l'argent.
     Exit: (23) argent(jean) ? creep % Jean a de l'argent.
     Call: (23) vacances(jean) ? creep % Vérification si Jean est en vacances.
     Call: (24) mois(aout) ? creep % Vérification si on est en août.
     Fail: (24) mois(aout) ? creep % Faux, on est en juillet.
     Fail: (23) vacances(jean) ? creep % Jean n'est pas en vacances.
     Redo: (23) argent(_4675) ? creep % Essai avec un autre X.
     Exit: (23) argent(alain) ? creep % Alain a de l'argent.
     Call: (23) vacances(alain) ? creep % Vérification si Alain est en vacances.
     Call: (24) mois(juillet) ? creep % Vérification si on est en juillet.
     Exit: (24) mois(juillet) ? creep % Vrai.
     Exit: (23) vacances(alain) ? creep % Alain est en vacances.
     Call: (23) soleil ? creep % Vérification s'il y a du soleil.
     Call: (24) mois(aout) ? creep % Vérification si on est en août.
     Fail: (24) mois(aout) ? creep % Faux.
     Fail: (23) soleil ? creep % Pas de soleil.
     Redo: (22) bonne_humeur(_3424) ? creep % Essai avec la deuxième condition.
     Call: (23) reussite_travail(_3424) ? creep % Vérification de la réussite au travail.
     Exit: (23) reussite_travail(jean) ? creep % Jean réussit au travail.
     Call: (23) reussite_famille(jean) ? creep % Vérifier la réussite familiale de Jean.
     Fail: (23) reussite_famille(jean) ? creep % Faux.
     Redo: (23) reussite_travail(_3424) ? creep % Essai avec un autre X.
     Exit: (23) reussite_travail(alain) ? creep % Alain réussit au travail.
     Call: (23) reussite_famille(alain) ? creep % Vérifier la réussite familiale d'Alain.
     Exit: (23) reussite_famille(alain) ? creep % Vrai.
     Exit: (22) bonne_humeur(alain) ? creep % Alain est de bonne humeur.

X = alain. % Résultat final.
```
### Arbre
```java
bonne_humeur(X)
├── Condition 1 (argent + vacances + soleil)
│   ├── X = jean
│   │   ├── argent(jean) → ✅
│   │   ├── vacances(jean) → ❌ (nécessite mois(aout), or mois(juillet) vrai)
│   │   └── Échec.
│   └── X = alain
│       ├── argent(alain) → ✅
│       ├── vacances(alain) → ✅ (mois(juillet) est vrai)
│       ├── soleil → ❌ (mois(aout) requis, or mois(juillet) vrai)
│       └── Échec.
└── Condition 2 (réussite travail + famille)
    ├── X = jean
    │   ├── reussite_travail(jean) → ✅
    │   ├── reussite_famille(jean) → ❌
    │   └── Échec.
    └── X = alain
        ├── reussite_travail(alain) → ✅
        ├── reussite_famille(alain) → ✅
        └── ✅ Solution : X = alain.
```

# Exercise 3
### Execution 
```prolog
?- programme.
Combien de nombres voulez-vous entrer ? 6.
Entrez un nombre : |: 1.
Entrez un nombre : |: 2
Entrez un nombre : |: 3.
Entrez un nombre : |: 4.
Entrez un nombre : |: 5.
Entrez un nombre : |: 6.
Somme des nombres : 21 
Maximum des nombres : 6 
true.
```