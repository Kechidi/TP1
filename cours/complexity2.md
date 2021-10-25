# Complexité des algorithmes 2

## Analyse du tri par fusion

Un algorithme basé sur le principe « diviser pour régner » :

  * Diviser le problème initial en plusieurs sous-problèmes plus faciles à résoudre (de façon récursive).

  * Combiner les solutions des sous-problèmes pour obtenir la solution du problème initial.

Rappel de l'algorithme sur un exemple : `[5, 2, 4, 6, 1, 3, 2, 6]`

```
ALGORITHME TriFusion(a, debut, fin)
ENTRÉE :
  a - un tableau
  debut - indice du premier élément
  fin - indice du dernier élément
SORTIE :
  Le sous-tableau a[debut ... fin] est trié
TAILLE :
  fin - debut + 1

DEBUT
  SI debut < fin ALORS
    milieu <- (debut + fin) / 2
    TriFusion(a, debut, milieu)
    TriFusion(a, milieu + 1, fin)
    Fusion(a, debut, milieu, fin)
  FINSI
FIN     
```

```
ALGORITHME Fusion(a, debut, milieu, fin)
ENTRÉE :
  a - un tableau
  debut <= milieu <= fin : indices dans le tableau
  les sous-tableaux a[debut ... milieu] et a [milieu + 1 ... fin] sont triés
  SORTIE :
    Le sous-tableau a[debut ... fin] est trié
  TAILLE :
    fin - debut + 1

DEBUT
  i <- debut
  j <- milieu + 1
  k <- 0

  TANTQUE i <= milieu et j <= fin FAIRE
    SI a[i] < a[j] ALORS
      tmp[k] <- a[i]
      i <- i + 1
    SINON
      tmp[k] <- a[j]
      j <- j + 1
    FINSI
    k <- k + 1
  FINTANTQUE

  TANTQUE i <= milieu
    tmp[k] <- a[i]
    i <- i + 1
    k <- k + 1
  FINTANTQUE

  TANTQUE j <= fin
    tmp[k] <- a[j]
    j <- j + 1
    k <- k + 1
  FINTANTQUE

  POUR i de debut à fin FAIRE
    a[i] <- tmp[i - debut]
  FINPOUR
FIN
```

Soit $`T(n)`$ le temps d'exécution de l'algorithme `TriFusion` pour une entrée de taille $`n`$. On peut facilement voir que

```math
T(n) =
\begin{cases}
  c & \text{pour $n = 1$} \\
  2 T(\frac{n}{2}) + T_\text{f}(n) & \text{pour $n > 1$}
\end{cases}
```

où $`T_\text{f}(n)`$ est le temps d'exécution de l'algorithme `Fusion`. En ce qui concerne ce dernier, on constate que dans les trois boucles `TANTQUE` il y a une itération par élément du tableau. Pareil pour la dernière boucle `POUR`. On a donc

```math
T_\text{f}(n) = \Theta(n) = a n + b
```

---

**Exercice** Montrer que $`T(n) = \Theta(n \log n)`$.

  * Commencer par poser $`n = 2^k`$
  * Montrer par récurrence que
    ```math
    T(2^k) = 2^k c + a k 2^k + (2^k - 1) b
    ```
  * En conclure que $`T(n) = \Theta(n \log n)`$

---

À noter qu'on peut omettre la base du logarithme car le changement de base revient à la multiplication par une constante.

---

**Exercice** Nous avons deux ordinateurs :

  * Ordinateur A capable d'exécuter $`10^9`$ (1 milliard) d'instructions par seconde
  * Ordinateur B capable d'exécuter $`10^7`$ (10 millions) d'instructions par seconde

A est donc 100 fois plus rapide que B.

Nous avons deux algorithmes de tri :

  * Tri par insertion implémenté en un langage de bas niveau et très optimisé. Pour une entrée de taille $`n`$ il exécute $`2 n^2`$ instructions au pire des cas.
  * Tri par fusion implémenté en un langage interprété et pas du tout optimisé. Il exécute $`50 n \log_2 n`$ instructions pour une entrée de taille $`n`$.

On exécute le tri par insertion sur l'ordinateur rapide A et le tri par fusion sur l'ordinateur lent B.

Estimer le temps d'exécution des deux algorithmes pour des tableaux de taille 1 million et 10 millions.

---
