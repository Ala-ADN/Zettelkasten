$$Ala\ eddine\ Zaouali$$
$$Aya\ Belkacem$$
## I/ Exercice 1  

### Tester la position de Dimitri  
```prolog
?- dimitri(1,2). 
false.
?- dimitri(5,4).
true.
```

### Tester la présence d'un arbre  
```prolog
?- arbre(1,1).
true.
?- arbre(1,2).
false.
```

### Tester la présence d'un rocher  
```prolog
?- rocher(2,3).
true.
?- rocher(2,4).
false.
```

### Trouver la vache zombie  
```prolog
?- vache(X,Y,zombie).
X = 7,
Y = 3.
```

### Vérifier que la vache est vivante  
```prolog
?- vache(2,2,vivante).
true.
```

---

## II/ Exercice 2  

### Vérification des places occupées  
```prolog
?- occupe(2,3).
true.
?- occupe(2,6).
false.
```

### Trouver des places libres  
```prolog
?- libre(X,Y).
X = 0,
Y = 12.
?- libre(X,Y).
X = 2,
Y = 1.
```

### Placer un arbre  
```prolog
?- placer_arbres(2).
true ;
false.
```
Après exécution, de nouveaux arbres apparaissent dans la base de faits :
```prolog
?- arbre(X,Y).
X = Y, Y = 1 ;
X = 8,
Y = 5 ;
X = 3,
Y = 6 ;
X = Y, Y = 0 ;
X = 8,
Y = 10.
```

### Placer des vaches  
```prolog
?- placer_vaches(simmental, 2).
true.
```
Liste des vaches après placement :
```prolog
?- listing(vache).
:- dynamic vache/4.

vache(2, 2, brune, vivante).
vache(3, 4, simmental, vivante).
vache(8, 6, alpine_herens, vivante).
vache(7, 3, simmental, zombie).
vache(2, 11, simmental, vivante).
vache(1, 8, simmental, vivante).

true.
```

### Placer Dimitri  
```prolog
?- placer_dimitri.
true.
?- dimitri(X,Y).
X = 5,
Y = 4.
```

### Placer des rochers  
```prolog
?- placer_rochers(2).
true.
```
Liste des rochers après placement :
```prolog
?- listing(rocher).
:- dynamic rocher/2.

rocher(2, 3).
rocher(7, 6).
rocher(4, 4).
rocher(5, 5).
rocher(6, 9).
rocher(6, 13).

true.
```

### Tester la liste des positions des vaches  
```prolog
?- vaches(L).
L = [(2, 2, brune, vivante), (3, 4, simmental, vivante), (8, 6, alpine_herens, vivante), (7, 3, simmental, zombie)].
```

### Tester créer un zombie  
Une vache vivante est transformée en zombie aléatoirement :
```prolog
?- vaches(L).
L = [(2, 2, brune, vivante), (8, 6, alpine_herens, vivante), (7, 3, simmental, zombie), (3, 4, simmental, zombie)].
```

---

## III/ Exercice 3  

### Tester la question de direction  
```prolog
?- question(R).
Dans quelle direction voulez-vous déplacer Dimitri ? Entrez: nord, sud, est, ouest, ou reste:nord.
R = nord.
```

### Tester la zombification  
Après ajout d'une vache vivante près d'une zombie :
```prolog
?- assert(vache(7, 4, brune, vivante)).
true.
```
Exécution de zombification :
```prolog
?- zombification.
false.
```
La vache devient zombie :
```prolog
?- listing(vache).
:- dynamic vache/4.

vache(2, 2, brune, vivante).
vache(3, 4, simmental, vivante).
vache(8, 6, alpine_herens, vivante).
vache(7, 3, simmental, zombie).
vache(7, 4, brune, zombie).

true.
```

### Déplacement de la vache  
Avant déplacement :
```prolog
?- listing(vache).
vache(brune, 3, 3, vivante).
```
Après déplacement vers le nord :
```prolog
?- deplacement_vache(3, 3, nord).
true.
?- listing(vache).
vache(brune, 3, 2, vivante).
```

### Tester déplacement joueur  
Avant déplacement :
```prolog
?- listing(dimitri).
:- dynamic dimitri/2.

dimitri(5, 4).

true.
```
Après déplacement vers le nord :
```prolog
?- deplacement_joueur(nord).
true.
?- dimitri(X,Y).
X = 5,
Y = 3.
```

### Vérification  
Ajout d'une vache zombie près de Dimitri :
```prolog
?- assert(vache(brune, 5, 5, zombie)).
true.
```
Vérification :
```prolog
?- verification.
false.
```