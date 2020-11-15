# Arbres rouge-noire (ARN)

Les arbres équilibrés sont des ABR pour lesquels $`h = O(\log n)`$. Parmi les différents types d'arbres équilibrés on va s'intéresser aux ARN où chaque nœud possède une couleur.

## Définitions et propriétés

Pour simplifier les définitions et les algorithmes qui suivent, on va considérer les `null` comme des feuilles externes qu'on notera par ☒. Ainsi chaque nœud interne a deux fils.

---

**Définition** Un *arbre rouge-noir (ARN)* est un ABR tel que :
  1. Chaque nœud est soit rouge, soit noir.
  2. La racine est noire.
  3. Les feuilles ☒ sont noires.
  4. Si un nœud est rouge, alors ses deux fils sont noirs.
  5. Pour chaque nœud, tous les chemins le reliant à des feuilles contiennent le même nombre de nœuds noirs.

À partir de (4) et (5) on voit déjà que l'arbre est « équilibré » dans le sens où pour chaque sous-arbre, la branche la plus longue est au plus deux fois plus longue que la branche la plus courte.

---

**Définition** La *hauteur noire* d'un nœud $`x`$ (on note $`hn(x)`$) est le nombre de nœuds noirs (sans compter $`x`$) sur le chemin de $`x`$ vers une feuille. La hauteur noire d'un arbre est la hauteur noire de sa racine.

---

**Exemple**

```mermaid
graph TD
  50((50)) --- 32 & 80
  32((32)) --- 26 & 40
  80((80)) --- 58 & 92
  26((26)) --- 18 & 30
  40((40)) --- 36 & 44
  58((58)) --- 54 & 74
  92((92)) --- s01[ ] & s02[ ]
  18((18)) --- 12 & 22
  30((30)) --- 28 & s03[ ]
  36((36)) --- s04[ ] & 38
  44((44)) --- s05[ ] & s06[ ]
  54((54)) --- s07[ ] & s08[ ]
  74((74)) --- 68 & 76
  12((12)) --- 4 & s09[ ]
  22((22)) --- s10[ ] & s11[ ]
  28((28)) --- s12[ ] & s13[ ]
  38((38)) --- s14[ ] & s15[ ]
  68((68)) --- s16[ ] & s17[ ]
  76((76)) --- s18[ ] & s19[ ]
  4((4)) --- s20[ ] & s21[ ]


  classDef black fill:#000, stroke:#fff;
  classDef red fill:#f00, stroke:#fff;
  class 50,80,26,40,92,30,36,44,54,74,s01,s02,12,22,s03,s04,s05,s06,s07,s08,s09,s10,s11,s12,s13,s14,s15,s16,s17,s18,s19,s20,s21 black;
  class 32,58,18,28,38,68,76,4 red;
```

---

En plus d'attributs `cle`, `pere`, `gauche` et `droit` des nœuds d'un ABR, on ajoute un attribut supplémentaire `couleur` (1 bit) qui peut prendre les valeurs `N` ou `R`.

Le fait de remplacer `null` par des nœuds ☒ noirs nous permet de simplifier nos algorithmes en écrivant par exemple

```java
if (x.couleur == N) { ... }
```

au lieu de

```java
if (x == null || x.couleur == N) { ... }
```

De l'autre coté, si on utilise un nœud différent pour chaque feuille, on gaspille de la mémoire. C'est pourquoi on peut remplacer tous les ☒ par un nœud unique, la *sentinelle*.

```mermaid
graph TD
  r(( ))
  r -.- f1(( )) & f2(( )) & f3(( ))
  r & f1 & f2 & f3 --> sentinelle[ ]

  style sentinelle fill:#000;
```

---

**Proposition** Pour tout nœud $`x`$, le sous-arbre de racine $`x`$ contient au moins $`2^{hn(x)} - 1`$ nœuds internes.


À démontrer par récurrence sur $`h(x)`$, la hauteur du sous-arbre de racine $`x`$.

---

**Proposition** La hauteur d'un ARN ayant $`n`$ nœuds internes est au plus $`2\log_2(n + 1)`$.

---

**Corollaire** Les opérations `rechercher()`, `minimum()`, `maximum()`, `successeur()` et `predecesseur()` s'exécutent en $`O(\log n)`$.

---

## Rotations

Pour ajouter ou supprimer des clés, on va se baser sur les opérations `ajouter()` et supprimer des ABR. Celles-ci ne préservent pas forcement les propriétés (1)-(5). Si certaines de ces propriétés sont violées, on va les réparer en faisant des recoloriages et des *rotations*.

```mermaid
flowchart TD
  subgraph s1[ ]
  x1((x)) --- a1[/a\] & y1((y))
  y1 --- b1[/b\] & c1[/c\]
  end

  subgraph s2[ ]
  y2((y)) --- x2((x)) & c2[/c\]
  x2 --- a[/a\] & b[/b\]
  end
```
