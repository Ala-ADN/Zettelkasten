$$Ala\ eddine\ Zaouali$$
$$Aya\ Belkacem$$
## Exercice 1 : Coloriage de carte
 *en 1976, deux Américains, Kenneth Appel et Wolfgang Haken, affirment avoir démontré le théorème des quatre couleurs Leur démonstration divise la communauté scientifique : pour la première fois, en effet, la démonstration exige l'usage de l'ordinateur pour étudier les 1 478 cas critiques*
### 1. Définition des régions adjacentes
```prolog
% Symétrie
adjacent(X, Y) :- adjacent(Y, X).

% Adjacence
adjacent(a, b).
adjacent(a, c).
adjacent(a, d).
adjacent(b, c).
adjecent(b, e).
adjacent(c, d).
adjacent(c, e).
adjecent(d, e).
```
### 2. Exemples de coloriage
```prolog
% Coloriage sans conflits
coloriageValide([
    color(a, v),
    color(b, b),
    color(c, r),
    color(d, b),
    color(e, v)
]).

% Coloriage avec conflits
coloriageInvalide([
    color(a, r),
    color(b, r), % Conflit
    color(c, v),
    color(d, b),
    color(e, v) % Conflit
]).
```
### 3. Détection des conflits
```prolog
% Vérifie si deux régions adjacentes ont la même couleur
conflit(X, Y, Coloriage) :-
    adjacent(X, Y),
    member(color(X, C), Coloriage),
    member(color(Y, C), Coloriage).

% Vérifie si un coloriage contient des conflits
conflit(Coloriage) :-
    adjacent(X, Y),
    member(color(X, C), Coloriage),
    member(color(Y, C), Coloriage).
```
### Exemples d'exécution
```prolog
% Test avec coloriage valide
?- coloriageValide(C), conflit(C).
false.

% Test avec coloriage invalide
?- coloriageInvalide(C), conflit(C).
true ;
true ;
false.
```
### Arbre de raisonnement
```mermaid
graph TD
    A["conflit(coloriageInvalide)"] --> B["adjacent(a,b)"]
    A --> C["adjacent(b,c)"]
    A --> D["adjacent(c,e)"]
    B --> E["color(a,r) et color(b,r) ?"]
    E -->|Oui| F["Conflit trouvé"]
    C --> G["color(b,r) et color(c,v) ?"]
    G -->|Non| H["Pas de conflit"]
    D --> I["color(c,v) et color(e,v) ?"]
    I -->|Oui| J["Conflit trouvé"]
```

---
## Exercice 2 : Suite de Fibonacci

### 1. Implémentation récursive
```prolog
fibonacci(0, 1). % Cas de base U0
fibonacci(1, 1). % Cas de base U1
fibonacci(N, F) :-
    N > 1,
    N1 is N - 1,
    N2 is N - 2,
    fibonacci(N1, F1),
    fibonacci(N2, F2),
    F is F1 + F2.
```
### Exemple d'exécution
```prolog
?- fibonacci(3, F).
F = 3 ;
false.
```
### Arbre de résolution
```mermaid
graph TD
    A["fibonacci(3,F)"] --> B["N > 1 ?"]
    B -->|Oui| C["N1 = 2, N2 = 1"]
    C --> D["fibonacci(2,F1)"]
    D --> E["N > 1 ?"]
    E -->|Oui| F["N1 = 1, N2 = 0"]
    F --> G["fibonacci(1,1) --> F1a=1"]
    F --> H["fibonacci(0,1) --> F2a=1"]
    D --> I["F1 = 1+1 = 2"]
    C --> J["fibonacci(1,F2) --> F2=1"]
    A --> K["F = 2+1 = 3"]
```

---

## Exercice 3 : Calcul factoriel

### 1. Implémentation récursive
```prolog
factorielle(0, 1). % Cas de base
factorielle(N, F) :-
    N > 0,
    N1 is N - 1,
    factorielle(N1, F1),
    F is N * F1.
```

### Exemple d'exécution
```prolog
?- factorielle(3, F).
F = 6 ;
false.
```

### Arbre de raisonnement
```mermaid
graph TD
    A["factorielle(3, F)"] --> B{"N > 0 ?"}
    B -->|Oui| C["N1 = 2"]
    C --> D["factorielle(2, F1)"]
    D --> E{"N > 0 ?"}
    E -->|Oui| F["N1 = 1"]
    F --> G["factorielle(1, F1a)"]
    G --> H{"N > 0 ?"}
    H -->|Oui| I["N1 = 0"]
    I --> J["factorielle(0, 1) → F1b=1"]
    G --> K["F1a = 1 * 1 = 1"]
    D --> L["F1 = 2 * 1 = 2"]
    A --> M["F = 3 * 2 = 6"]
```
