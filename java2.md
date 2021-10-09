# Rappels de Java 2

## Classes abstraites

  * permettent de partager du code entre des classes étroitement liées
  * implémentent une partie des fonctionnalités et délèguent une partie aux sous-classes

```java
abstract class Horloge {
  private int heure, minute;

  public Horloge(int heure, int minute) {
    this.heure = (heure + minute / 60) % 24;
    this.minute = minute % 60;
  }

  public int getHeure() {
    return heure;
  }

  public int getMinute() {
    return minute;
  }

  public void tick() {
    if (++minute == 60) {
      minute = 0;
      heure = (heure + 1) % 24;
    }
  }

  public abstract void dessiner();
}


class HorlogeAnalogique extends Horloge {
  public HorlogeAnalogique(int heure, int minute) {
    super(heure, minute);
  }

  public void dessiner() {
    ...
  }
}

class HorlogeNumerique extends Horloge {
  ...
}
```

```java
Horloge h = new Horloge(10, 15); // ERREUR

List<Horloge> horloges = new ArrayList<Horloge>();
horloges.add(new HorlogeAnalogique(12, 15));
horloges.add(new HorlogeNumerique(18, 15));
...
for (Horloge h : horloges) h.dessiner();
```

## Interfaces

```java
abstract class Audible {
  public void ecouter();
}

class HorlogeAudible extends Horloge, Audible { // Pas d'héritage multiple
  ...
}
```

```java
interface Audible {
  void ecouter();
}

class HorlogeAudible extends Horloge implements Audible {
  ...

  public void dessiner() {
    ...
  }

  public void ecouter() {
    ...
  }
}
```

*Java < 8 :*
  * classes purement abstraites
  * pas d'attributs
  * pas de constructeurs
  * toutes les méthodes sont publiques

```java
interface Mobile {
  void marcher();
  void voler();
}
```

*Problème :* Les interfaces ne peuvent pas évoluer facilement.

*Solution :* À partir de Java 8 on peut avoir des implémentations

```java
interface Mobile {
  void marcher();
  void voler();
  default void nager() {
    System.out.println("Au secours !");
  }
}
```

Toujours polymorphisme :

```java
List<Audible> audibles = new ArrayList<>();
audibles.add(new HorlogeAudible(18, 40));
audibles.add(new Chanson("Daft Punk", "Technologic"));
...
for (Audible a : audibles) a.ecouter();
```
