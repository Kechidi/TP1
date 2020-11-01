# Complexité des algorithmes

Rappel sur le tri par insertion :

1. Le cas le plus favorable : tableau trié.

    $`T(n) = a n + b`$ - fonction linéaire

2. Le cas le plus défavorable : tableau trié en sens inverse

    $`T(n) = a' n^2 + b' n + c'`$ - fonction quadratique

3. Le cas moyen

    $`T(n) = a'' n^2 + b'' n + c''`$ - fonction quadratique


Le plus souvent on considère le temps d'exécution dans le pire des cas parce que :

  * L'algorithme ne prendra jamais plus de temps
  * Le pire des cas arrive assez souvent
  * Souvent le cas moyen est aussi mauvais que le pire des cas (temps d'exécution du même ordre)


## Ordre de grandeur

Rappelons la démarche qu'on a suivi :

1. Ignorer les temps réels d'exécution en les remplaçant par les coûts abstraites $`c_i`$

2. Ignorer les coûts abstraites $`c_i`$ :

  $`T(n) = a n^2 + b n + c`$

3. Éliminer les termes d'ordre inférieur et ignorer le coefficient du terme d'ordre supérieur :

  $`T(n) = \Theta(n^2)`$ (ordre de grandeur quadratique)

L'analyse asymptotique est un moyen de comparer les performances *relatives* des algorithmes.

---

**Exemple** Considérons trois algorithmes :

  * A1 de complexité $`\Theta(n^2)`$
  * A2 de complexité $`\Theta(n^3)`$
  * A3 de complexité $`\Theta(n^2)`$

On peut dire que A1 est meilleur que A2 *pour les entrés de grande taille*. Mais on ne peut pas dire qui est le meilleur parmi A1 et A3, pour cela il faut une analyse plus fine.

---

**Exemple** Les termes d'ordre inférieur sont peu importants *pour les entrées de grande taille*

```math
T(n) = 0,1 n^2 + 1000 n + 100000
```

Pour $`n = 10`$ le terme linéaire est 1000 fois plus grand que le terme quadratique. Mais pour $`n = 1\;000\;000`$ le terme linéaire est 100 fois *plus petit* que le terme quadratique. Plus $`n`$ est grand, plus les termes d'ordre supérieur sont importants, quels que soient les coefficients.

---


## Notations asymptotiques

---

**Définition** Soit $`g(n)`$ une fonction.

$`\Theta(g(n))`$ est *l'ensemble* des fonctions $`f(n)`$ pour lesquelles il existe des constantes positives $`c_1, c_2, n_0`$, telles que :

```math
0 \le c_1 g(n) \le f(n) \le c_2 g(n) \text{ pour tout } n \ge n_0
```

On dit aussi que $`f`$ est « prise en sandwich » entre $`c_1 g`$ et $`c_2 g`$ pour $`n`$ assez grand ou encore que $`f`$ est égale à $`g`$ à un facteur constant près.

---

$`\Theta(g(n))`$ est un ensemble de fonctions et la notation mathématiquement correcte est $`f(n) \in \Theta(g(n))`$ mais on note $`f(n) = \Theta(g(n))`$ par abus.

---

**Exercice** Montrer que :

  * $`\frac{1}{2} n^2 - 3 n = \Theta(n^2)`$
  * $`6 n^3 \neq \Theta(n^2)`$

---

**Définition** Soit $`g(n)`$ une fonction.

$`O(g(n))`$ est *l'ensemble* des fonctions $`f(n)`$ pour lesquelles il existe des constantes positives $`c, n_0`$, telles que :

```math
0 \le f(n) \le c g(n) \text{ pour tout } n \ge n_0
```

---

Il est évident que si $`f(n) = \Theta(g(n))`$ alors $`f(n) = O(g(n))`$ et donc $`\Theta(g(n)) \subseteq O(g(n))`$ mais l’inverse n'est pas vrai.

---

**Exercice** Montrer que $`n^2 = O(n^3)`$  mais $`n^2 \neq O(n^3)`$

---

Souvent on parle de la complexité des algorithmes en termes de $`O`$ et pas de $`\Theta`$. On peut dire que notre algorithme de tri par insertion s'exécute *toujours* en $`O(n^2)`$ et en $`\Theta(n^2)`$ *au pire des cas*.


**Propriétés**

  * Transitivité : si $`f(n) = \Theta(g(n))`$ et $`g(n) = \Theta(h(n))`$ alors $`f(n) = \Theta(h(n))`$. Idem pour $`O`$.

  * Réflexivité : $`f(n) = \Theta(f(n))`$. Idem pour $`O`$.

  * Symétrie : $`f(n) = \Theta(g(n)) \text{ ssi } g(n) = \Theta(f(n))`$. Pas de symétrie pour $`O`$, pourquoi ?

  * $`\sum_{i = 0}^{d} a_i n^i = \Theta(n^d)`$

## Analyse du tri par fusion

Un algorithme basé sur le principe « diviser pour régner » :

  * Diviser le problème initial en plusieurs sous-problèmes plus faciles à résoudre (de façon récursive).

  * Combiner les solutions des sous-problèmes pour obtenir la solution du problème initial.

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
  *En conclure que $`T(n) = \Theta(n \log n)`$

---

À noter qu'on peut omettre la base du logarithme car le changement de base revient à la multiplication par une constante.

---

**Exercice** Nous avons deux ordinateurs :

  * Ordinateur A capable d'exécuter $`10^9`$ (1 milliard) d'instructions par seconde
  * Ordinateur B capable d'exécuter $`10^7`$ (10 millions) d'instructions par seconde

A est donc 100 fois plus rapide que B.

Le tri par insertion est implémenté en un langage de bas niveau et très optimisé. Pour une entrée de taille $`n`$ il exécute $`2 n^2`$ instructions au pire des cas.

Le tri par fusion est implémenté en un langage interprété et pas du tout optimisé. Il exécute $`50 n \log_2 n`$ instructions pour une entrée de taille $`n`$.

On exécute le tri par insertion sur l'ordinateur rapide A et le tri par fusion sur l'ordinateur lent B.

Estimer le temps d'exécution des deux algorithmes pour des tableaux de taille 1 million et 10 millions.
---
