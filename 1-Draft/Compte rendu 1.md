$$Ala\ Eddine\ Zaouali$$
$$Aya\ Belkacem$$```shell
PS C:\INSAT\TP_Logique\> swipl -s base.pl
Welcome to SWI-Prolog (threaded, 64 bits, version 9.2.9)
SWI-Prolog comes with ABSOLUTELY NO WARRANTY. This is free software.
Please run ?- license. for legal details.
For online help, visit https://www.swi-prolog.org

% base.pl compiled 0.00 sec, 2,048 bytes
```
# Exercise 1 :
1 ?-homme(jean).
true.

2 ?-homme(martine).
false.

3 ?-femme(lucie).
true.

4 ?-femme(laura).
false.

5 ?-homme(X).
X = jean ;
X = alain.

6 ?-femme(X).
X = lucie ;
X = nelly ;
X = martine.

7 ?-parent(jean, X).
X = lucie.

8 ?-parent(X, martine).
X = alain.

9 ?-enfant(X, Y).
X = lucie, Y = jean ;
X = lucie, Y = nelly ;
X = alain, Y = lucie ;
X = martine, Y = alain.

10 ?-mere(nelly, lucie).
true.

11 ?-mere(X, Y).
X = nelly, Y = lucie ;
X = lucie, Y = alain.

12 ?-ancetre(jean, martine).
true.

13 ?-ancetre(X, martine).
X = alain ;
X = lucie ;
X = jean ;
X = nelly.
```
# Exercise 2:
```
18 ?-carre.
donner un entier : 9.
Le carré de 9 est 81.
true.

19 ?-boucle.
donner un entier (0 pour quitter) : 10.
Le carré de 10 est 100.
donner un entier (0 pour quitter) : 8.
Le carré de 8 est 64.
donner un entier (0 pour quitter) : 4.
Le carré de 4 est 16.
donner un entier (0 pour quitter) : 1.
Le carré de 1 est 1.
Fin du programme.
true.

20 ?-factorielle(test).
donner un entier (0 pour quitter) : 12.
La factorielle de 12 est 479001600.
true.

21 ?-factorielle(test).
donner un entier (0 pour quitter) : 10.
La factorielle de 10 est 3628800.
true.

22 ?-halt.
```