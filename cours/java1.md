# Rappels de Java et comparaisons avec C++

Caractéristiques :
  * orienté objet (OO)
  * universel (general purpose)
  * propriétaire mais à license libre
  * machine virtuelle
  * syntaxe proche de C++
  * bibliothéque standard (E/S, structures de données, IHM etc.)
  * pas de pointeurs
  * pas d'héritage multiple (mais interfaces)
  * généricité

## Compilateur et machine virtuelle

```
javac Toto.java -> Toto.class (bytecode)
java Toto -> lancement de la VM
```

Contrairement à C++, pas d'édition de liens et pas d'exécutable natif.
  * plus de portabilité
  * moins d'efficacité (mais JIT)
  * il faut une machine virtuelle

## Variables

  * variables d'instance (champs, attributs)
  * variables de classe (`static`)
  * variables locales

### Types de base

  * `byte` (1 octet), `short` (2 octets), `int` (4 octets), `long` (8 octets)
  * `float` (4 octets) et `double` (8 octets) : arithmétique virgule flotante IEEE 754
  * `char` (2 octets)
  * `boolean`

### Références (tableaux, classes, interfaces)

Qu'ffiche le code suivant ?

```java
int x, y;
x = 2;
y = x;
y++;
System.out.println(x + " " + y);
```

Maintenant la même chose mais avec références :

```java
class MonInt {
  int val;

  MonInt(int val) {
    this.val = val;
  }

  int get() {
    return val;
  }

  void inc() {
    val++;
  }
}
...
MonInt mx, my;
mx = new MonInt(2);
my = mx;
my.inc();
System.out.println(mx.get() + " " + my.get());
```

**Rappel :** toutes les références en Java résident dans le *tas*.

Qu'en est-il du code équivalent en C++ ?

```cpp
MonInt mx(2), my;
my = mx;
my.inc();
cout << mx.get() << " " << my.get() << endl;
```

### Visibilité

  * `private` - dans la classe
  * par défaut - dans le package
  * `protected` - dans le package et les sous-classes
  * `public` - partout

## Le cycle de vie des objets

### Création

```java
MonInt mx; // contrairement à C++, pas d'objet crée
MonInt my = new MonInt(2); // création d'objet dans le *tas*
```

### Utilisation

```java
mx.inc(); // Erreur
my.inc(); // OK
```

### Recyclage

  * pas d'opérateur `delete`
  * recyclage automatique (Garbage Collector)

Sans GC on se retrouvrait très rapidement avec un tas saturé :

```java
for (int i = 0; i < 1_000_000_000; i++) {
  GrosObjet go = new GrosObjet();
  ...
}
```

Comment détecter si la mémoire occupé par un obet peut être libéré ?

**Approche 1** Recycler à la fin de la portée :

```java
{
  ...
  Truc a = new Truc();
  ...
  // Fin de la portée de a
  // Peut-on recycler la mémoire ?
}
```

**Approche 2** Maintenir un compteur des rérérences actives pour chaque objet. Recycler lorsque ce compteur tombe à zéro. Est-ce que cela marche ?

Le travail de GC est complexe et coûte cher. Parfois des petits « gestes écolo » peuvent l'aider :

```java
{
  GrosObjet go = new GrosObjet();
  ...
  // je n'ai plus besoin de go mas GC ne peut pas récupérer la mémoire
  go = null;
  // maintenant il peut
  ...
}
```

#### Méthode `finalize()` (deprecated)

```java
class Object {
  protected void finalize() {}
}
```
Cette méthode est appelée par GC quand il décide de recycler la mémoire occupée par un objet. Les classes peuvent la surcharger.

**Exemple**

```java
public class MonInt {
  private static int nbObjets = 0;

  private int val;

  public MonInt() {
    this(0);
  }

  public MonInt(int val) {
    nbObjets++;
    this.val = val;
  }

  protected void finalize() {
    nbObjets--;
  }

  public int get() {
    return val;
  }

  public static void main(String[] args) {
    int s = 0;
    for (int i = 0; i < 20_000_000; i++) {
      MonInt mx = new MonInt(i);
      s += mx.get();
      if (i % 1000 == 0) System.out.println(i + " " + MonInt.nbObjets);
    }
  }
}
```
