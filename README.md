# Wiki

**Paterns étudié :**

1. Objet composite
2. Injection de contrôle/dépendance

## Objet composite 

### Situation :

On a un problème, prenons deux produits et boîtes, une boîte contient plusieurs produits ainsi qu'un certain nombre de boîtes plus petites. Ces boîtes peuvent également contenir quelques produits ou même d'autre boîtes encore plus petites, et ainsi de suite 

![exemple des boites](https://refactoring.guru/images/patterns/diagrams/composite/problem-fr.png?id=16882f793d754179a18458b6426b36bb)

Une commande peut contenir divers produit a l'intérieur de boîtes, elles-mêmes rangées dans de plus grosses boîtes. La structure complète ressemble a un arbre.

![l'arbre](https://refactoring.guru/images/patterns/content/composite/composite.png?id=73bcf0d94db360b636cd745f710d19db)

### Solution a la situation :

Nous pouvons manipuler les produits et les boîtes à l'aide d'une interface qui déclare une méthode de calcul du prix total. 

C'est a dire que pour un produit on donne simplement son prix et pour une boîte, on parcourt chacun de ses objets, on leur demande leur prix, puis on donne un total pour la boîte. Si l'un de ces objets est une boîte plus petite, cette dernière va aussi parcourir son propre contenu et ainsi de suite, jusqu'à ce que tous les prix aient été calculés.

![Solution](https://refactoring.guru/images/patterns/content/composite/composite-comic-1-fr.png?id=b318eb1564d5ce4f75a66faa97e1ca6f)

### Qu'est-ce qu'un objet composite ?

Un objet composite ou pattern composite (patron de conception en français) permet de gérer un ensemble d'objet.
En effet en java il est facile de gerer un objet a la fois mais quand il faut en gérer plusieurs en même temps ou même gerer un ensemble d'objet ca peut poser problème.

### Comment l'utiliser en JAVA ?

![exemple avec un diagramme](https://miro.medium.com/proxy/1*FpmtB3L4DTIoZUgLYqjz2g.webp)

Le composant déclare l'interface des objets qui rentre et implémente leur comportements. Il définit aussi une interface pour accéder aux composants enfants et les modifier.

Le composite définit le comportement des composant qui ont un composants enfants et vas les stockers puis implémenter leur opération.

Le client lui vas utiliser l'interface définit par le composant pour pouvoir manipuler les objets.

La feuille n'a pas d'enfants et elle définit le comportement des objets avec opération().

### Exemple concret :

```
public interface Department {
	void printDepartmentName();
}
```

On définit une interface Departement qui contient une fonction qui affiche le nom des départements.

Ensuite on vas définir 2 classes qui vont contenir toute les deux la méthode pour afficher le nom des départements mais elles ne contiennent pas d'autre objet de la classe département.


```
public class FinancialDepartment implements Department {
	private Integer id;
	private String name;
	public void printDepartmentName() {
		System.out.println(getClass().getSimpleName());
	}
	// standard constructor, getters, setters
}
```

```
public class SalesDepartment implements Department {
	private Integer id;
	private String name;
	public void printDepartmentName() {
		System.out.println(getClass().getSimpleName());
	}
	// standard constructor, getters, setters
}
```

Puis enfin une classe composite qui vas contenir plusieur élément de la Class Departement et en plus en rajouter ou supprimer avec (addDepartment, removeDepartment) en plus de la méthode pour afficher les noms de département 

```
public class HeadDepartment implements Department {
	private Integer id;
	private String name;
	private List<department> childDepartments;</department>
	public HeadDepartment(Integer id, String name) {
		this.id = id;
		this.name = name;
		this.childDepartments = new ArrayList<>();
	}
	public void printDepartmentName() {
		childDepartments.forEach(Department::printDepartmentName);
	}
	public void addDepartment(Department department) {
		childDepartments.add(department);
	}
	public void removeDepartment(Department department) {
		childDepartments.remove(department);
	}
}
```


## Injection de contrôle/dépendance 

### Qu'est-ce qu'une Injection de contrôle de dépendance ?

C'est un patron de conception consistant à construire les dépendances d’un module hors de celui-ci, afin d’obtenir un maximum de contrôle. 
L'injection de dépendance est une forme de ce que l'on appelle inversion des contrôles. 

### Le principe de ce pattern

C’est une solution aux modules qui contrôlent tout eux-mêmes, et donc vous n’avez plus votre mot à dire. Le principe est donc simple , il dit qu’un service ne devrait jamais être construit à l’intérieur d’un client , mais injecté de l’extérieur. 

![Exemple avec une console](https://itexpert.fr/content/images/2021/03/4-1.png)

### Ce qu'il permet de faire

. De vous donner un contrôle TOTAL sur les données et le comportement du service. 

. D’appliquer et de profiter efficacement du principe d’inversion des dépendances. 

. De ne plus avoir à construire le service à l’intérieur du client, ce qui permet une meilleure séparation des responsabilités . 

. De vous débarrasser des longues listes de paramètres à n’en plus finir, car vous n’avez plus qu’un paramètre par service. 

. De faciliter les tests unitaires, car toutes les dépendances sont externalisées, ce qui permet de se concentrer sur l’essentiel. 

### Explication au niveau des classes Java 

![Diagramme](https://itexpert.fr/content/images/2021/03/6-1.png)

Prenons toujours l’exemple d’une console, on veut lancer un jeux, on fait alors une classe avec un constructeur qui prend en paramètre le service et on va le stocker dans un champ (privé de préférence). Ici on veut laisser au contexte (le code qui appelle le client) le choix du type du service (le jeux).

Il suffit donc de : 

. Réunir tous les types que l’on souhaite passer à notre client sous une interface commune

. De passer au client un objet du type de cette interface

### Application de cet exemple en code Java

Commençons par créer différents types et les placer sous la même interface

```
public interface INesVideoGame
{
    void Run();
}

public class SuperMarioBros : INesVideoGame
{
    public void Run() => Console.WriteLine("Playing Super Mario Bros");
}

public class Contra : INesVideoGame
{
    public void Run() => Console.WriteLine("Playing Contra");
}

public class DeadlyTowers : INesVideoGame
{
    public void Run() => Console.WriteLine("Suffering on Deadly Towers");
}
```
Ensuite, créons notre client. Celui-ci possède bien sûr un constructeur, mais aussi une méthode Start() qui lui permettra de lancer le jeu injecté.

```
public class NintendoEntertainmentSystem
{
    private INesVideoGame VideoGame;

    public NintendoEntertainmentSystem(INesVideoGame cartridge) =>
        VideoGame = cartridge;

    public void Start() => VideoGame?.Run();
}
```
Maintenant utilisons le avec une fonction Main() qui est ici le contexte d'une console 

```
static void Main(string[] args)
{
    NintendoEntertainmentSystem nes;

    var superMarioBrosCartridge = new SuperMarioBros();
    nes = new NintendoEntertainmentSystem(superMarioBrosCartridge);
    nes.Start();    // Playing Super Mario Bros

    var contraCartridge = new Contra();
    nes = new NintendoEntertainmentSystem(contraCartridge);
    nes.Start();    // Playing Contra

    var deadlyTowersCartridge = new DeadlyTowers();
    nes = new NintendoEntertainmentSystem(deadlyTowersCartridge);
    nes.Start();    // Suffering on Deadly Towers
}
```

### Conclusion


**Cas où on peut l'utiliser :**

. Avoir un contrôle total sur la configuration d’un service

. Altérer le comportement d’un client de l’extérieur

. Réduire le nombre de paramètres d’une fonction


**Cas où il vaut mieux ne pas l'utiliser :**

. Votre service est construit sans paramètre extérieur. Typiquement, les objets locaux et/ou temporaires

. Vous voulez limiter l’impact du contexte sur le service. Par exemple : si certaines données ne doivent pas être altérées hors du client




## Source :
*(https://www.ionos.fr/digitalguide/sites-internet/developpement-web/composite-pattern/)*
*(https://support.zendesk.com/hc/fr/articles/4408846544922-Formatage-de-texte-avec-Markdown)*
*(https://refactoring.guru/fr/design-patterns/composite)*
*(https://support.zendesk.com/hc/fr/articles/4408846544922-Formatage-de-texte-avec-Markdown)*
*(https://itexpert.fr/blog/injection-dependance/#explications)*


