# Complexité des algorithmes 1

## Introduction

Analyser un algorithme : prévoir les ressources nécessaires à son exécution. Les ressources peuvent être :
  * mémoire utilisée
  * bande passante
  * ...
  * temps d'exécution (le plus souvent)

Analyse asymptotique : moyen de comparer les performances relatives.

Pour analyser un algorithme, il nous faut un modèle de machine. On utilisera une *Random Access Machine* (RAM) :
  * mémoire centrale (temps d'accès constant)
  * processeur unique
  * jeu d'instructions (temps d'exécution constant)
  * exécution séquentielle

## Temps d'exécution

En général, le temps d'exécution dépend de la taille du problème : $`n \rightarrow T(n)`$. La taille peut être :
  * nombre d'éléments du tableau (tri)
  * nombre de bits (multiplication)
  * nombre d'arêtes et/ou sommets (graphes)
  * ...

Problème : le comportement d'un algorithme peut être très différent sur des entrées différentes (même de la même taille). C'est pourquoi on utilise :
  * temps d'exécution dans le pire des cas
  * temps d'exécution moyenne

## Analyse du tri par insertion

Rappel de l'algorithme sur un exemple : `[7, 6, 2, 8, 1, 4, 5]`

```
ALGORITHME TriInsertion
ENTREE :
  a - un tableau
SORTIE :
  a trié
TAILLE :
  n -le nombre d'éléments du tableau

DÉBUT
  POUR i de 1 à n - 1 FAIRE             // 1
    cle <- a[i]                         // 2
    j <- i - 1                          // 3
    TANTQUE j >= 0 et a[j] > cle FAIRE  // 4
      a[j + 1] <- a[j]                  // 5
      j <- j - 1                        // 6
    FINTANTQUE
    a[j + 1] <- cle                     // 7
  FINPOUR
FIN
```

Soit $`c_k`$ le coût (le temps d'exécution) de la ligne $`k`$. Soit $`t_i`$ le nombre d'exécutions du test de la boucle de ligne 4 pour un `i` donné.

---

**Exercice** Exprimer le temps d'exécution total en fonction de $`n`$, $`c_k`$ et $`t_i`$.

---


1. Le cas le plus favorable

    $`T(n) = a n + b = \Theta(n)`$ - fonction linéaire

2. Le cas le plus défavorable

    $`T(n) = a' n^2 + b' n + c' = \Theta(n^2)`$ - fonction quadratique

3. Le cas moyen

    $`T(n) = a'' n^2 + b'' n + c'' = \Theta(n^2)`$ - fonction quadratique


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

**Exercice** Montrer que $`n^2 = O(n^3)`$  mais $`n^2 \neq \Theta(n^3)`$

---

Souvent on parle de la complexité des algorithmes en termes de $`O`$ et pas de $`\Theta`$. On peut dire que notre algorithme de tri par insertion s'exécute *toujours* en $`O(n^2)`$ et en $`\Theta(n^2)`$ *au pire des cas*.


**Propriétés**

  * Transitivité : si $`f(n) = \Theta(g(n))`$ et $`g(n) = \Theta(h(n))`$ alors $`f(n) = \Theta(h(n))`$. Idem pour $`O`$.

  * Réflexivité : $`f(n) = \Theta(f(n))`$. Idem pour $`O`$.

  * Symétrie : $`f(n) = \Theta(g(n)) \text{ ssi } g(n) = \Theta(f(n))`$. Pas de symétrie pour $`O`$, pourquoi ?

  * $`\sum_{i = 0}^{d} a_i n^i = \Theta(n^d)`$
