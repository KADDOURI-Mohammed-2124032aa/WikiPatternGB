<div align="center"> WIKI </div>

**Paterns étudié :**

1. Objet composite
2. Injection de contrôle/dépendance

## Objet composite 

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

## Injection de contrôle/dépendance 

C'est un patron de conception consistant à construire les dépendances d’un module hors de celui-ci, afin d’obtenir un maximum de contrôle. 
L'injection de dépendance est une forme de ce que l'on appelle inversion des contrôles. 
