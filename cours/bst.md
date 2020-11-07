# Arbres binaires de recherche (ABR)

---

**Définition** Un ABR est un arbre binaire tel que pour tout nœud `x` :
  - si `y` est un nœud du sous-arbre gauche de `x` alors `y.cle <= x.cle`
  - si `y` est un nœud du sous-arbre droite de `x` alors `y.cle >= x.cle`

---

**Exemple** L'ensemble de clés `{3, 5, 9, 9, 13, 15}` peut être stocké dans un ABR de plusieurs façons différentes. En voici deux :

```mermaid
graph TD
  subgraph a
    9a((9)) --- 5a((5)) & 13a((13))
    5a --- 3a((3)) & 9aa((9))
    13a --- i1[ ] & 15a((13))
  end
  subgraph b
    3b((3)) --- i2[ ] & 5b((5))
    5b((5)) --- i3[ ] & 13b((13))
    13b --- 9b((9)) & 15b((15))
    9b --- 9bb((9)) & i4[ ]
  end

  classDef invisible fill:#0000, stroke:#0000;
  class i1,i2,i3,i4 invisible;

  linkStyle 4,6,8,13 stroke:#0000
```

## Parcours dans l'ordre des clés

Quel parcours permet de visiter les clés dans l'ordre ? Quelle est sa complexité ?
