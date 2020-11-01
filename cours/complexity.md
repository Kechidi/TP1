# Complexité des algorithmes

Rappel sur le tri d'insertion :

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

  $`T(n) = \Theta(n^2)` (ordre de grandeur quadratique)

L'analyse asymptotique est un moyen de comparer les performances *relatives* des algorithmes.

---

**Exemple 1.** Considérons trois algorithmes :

  * A1 de complexité $`\Theta(n^2)`$
  * A2 de complexité $`\Theta(n^3)`$
  * A3 de complexité $`\Theta(n^2)`$

On peut dire que A1 est meilleur que A2 *pour les entrés de grande taille*. Mais on ne peut pas dire qui est le meilleur parmi A1 et A3, pour cela il faut une analyse plus fine.

---

**Exemple 2.** Les termes d'ordre inférieur sont peu importants *pour les entrées de grande taille*

```math
T(n) = 0,1 n^2 + 1000 n + 100000
```

Pour $`n = 10`$ le terme linéaire est 1000 fois plus grand que le terme quadratique. Mais pour $`n = 1\;000\;000` le terme linéaire est 100 fois *plus petit* que le terme quadratique. Plus $`n`$ est grand, plus les termes d'ordre supérieur sont importants, quels que soient les coefficients.
