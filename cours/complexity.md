# Complexité des algorithmes

Rappel sur le tri d'insertion :

1. Le cas le plus favorable : tableau trié.

    $T(n) = a n + b$ - fonction linéaire

2. Le cas le plus défavorable : tableau trié en sens inverse

    $T(n) = a' n^2 + b' n + c$ - fonction quadratique

3. Le cas moyen

    $T(n) = a" n^2 + b" n + c$ - fonction quadratique


Le plus souvent on considère le temps d'exécution dans le pire des cas parce que :

  * L'algorithme ne prendra jamais plus de temps
  * Le pire des cas arrive assez souvent
  * Souvent le cas moyen est aussi mauvais que le pire des cas (temps d'exécution du même ordre)


## Ordre de grandeur

Rappelons la démarche qu'on a suivi :
