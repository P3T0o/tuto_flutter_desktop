# Tuto flutter desktop

Tuto qui sert à créer un projet flutter desktop MacOS rapidement.

## Getting Started

This project is a starting point for a Flutter application.

A few resources to get you started if this is your first Flutter project:

- [Lab: Write your first Flutter app](https://docs.flutter.dev/get-started/codelab)
- [Cookbook: Useful Flutter samples](https://docs.flutter.dev/cookbook)

For help getting started with Flutter development, view the
[online documentation](https://docs.flutter.dev/), which offers tutorials,
samples, guidance on mobile development, and a full API reference.

# Explication détaillée de l'application Flutter

Ce document décrit en détail le code d'une petite application MacOS réalisée avec Flutter. L'application génère des paires de mots aléatoires (grâce au package `english_words`) et permet à l'utilisateur de sauvegarder ses favoris. La gestion d'état se fait à l'aide du package `provider`. Ci-dessous, chaque partie du code est expliquée en détail.

---

## Table des matières

1. [Point d'entrée de l'application](#point-dentrée-de-lapplication)
2. [Configuration de l'application avec `MyApp`](#configuration-de-lapplication-avec-myapp)
3. [Gestion de l'état avec `MyAppState`](#gestion-de-letat-avec-myappstate)
4. [Structure de l'interface utilisateur](#structure-de-linterface-utilisateur)
    - [Navigation et page principale (`MyHomePage`)](#navigation-et-page-principale-myhomepage)
    - [La page de génération (`GeneratorPage`)](#la-page-de-génération-generatorpage)
    - [La page des favoris (`FavoritesPage`)](#la-page-des-favoris-favoritespage)
    - [Widget personnalisé : `BigCard`](#widget-personnalisé--bigcard)
5. [Conclusion](#conclusion)

---

## Point d'entrée de l'application

Le code commence par importer les packages nécessaires :

```dart
import 'package:english_words/english_words.dart';
import 'package:flutter/material.dart';
import 'package:provider/provider.dart';
```

Ensuite, la fonction `main()` est définie et lance l'application :

```dart
void main() {
  runApp(MyApp());
}
```

- **`runApp(MyApp())`** : C'est le point d'entrée de l'application Flutter. Il initialise l'interface en affichant le widget racine `MyApp`.

---

## Configuration de l'application avec `MyApp`

La classe `MyApp` est un widget stateless qui configure l'application en enveloppant l'ensemble de l'interface avec un provider pour la gestion d'état et en définissant le thème global.

```dart
class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return ChangeNotifierProvider(
      create: (context) => MyAppState(),
      child: MaterialApp(
        title: 'Tuto flutter',
        theme: ThemeData(
          useMaterial3: true,
          colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepOrange),
        ),
        home: MyHomePage(),
      ),
    );
  }
}
```

### Points clés :

- **`ChangeNotifierProvider`**
    - Fournit une instance de `MyAppState` à tous les widgets descendants.
    - Permet aux widgets de s'abonner aux changements d'état via `notifyListeners()`.

- **`MaterialApp`**
    - Définit le titre, le thème (ici basé sur Material 3 avec une palette dérivée d'une couleur de base) et la page d'accueil (`MyHomePage`).

---

## Gestion de l'état avec `MyAppState`

La classe `MyAppState` étend `ChangeNotifier` et centralise la gestion de l'état de l'application.

```dart
class MyAppState extends ChangeNotifier {
  var current = WordPair.random();

  void getNext() {
    current = WordPair.random();
    notifyListeners();
  }

  var favorites = <WordPair>[];

  void toggleFavorite() {
    if (favorites.contains(current)) {
      favorites.remove(current);
    } else {
      favorites.add(current);
    }
    notifyListeners();
  }
}
```

### Explications :

- **`current`**
    - Stocke la paire de mots actuellement affichée. Elle est initialisée avec une valeur aléatoire grâce à `WordPair.random()`.

- **Méthode `getNext()`**
    - Génère une nouvelle paire de mots aléatoire et met à jour l'état via `notifyListeners()`, ce qui permet aux widgets abonnés de se recharger.

- **`favorites`**
    - Liste qui stocke les paires de mots favorites.

- **Méthode `toggleFavorite()`**
    - Ajoute ou retire la paire de mots actuelle dans la liste `favorites` selon qu'elle y figure déjà ou non, puis notifie les widgets pour mettre à jour l'affichage.

---

## Structure de l'interface utilisateur

L'interface est organisée autour de plusieurs widgets principaux : une page principale avec navigation (`MyHomePage`), une page de génération de mots (`GeneratorPage`), une page pour afficher les favoris (`FavoritesPage`) et un widget personnalisé pour afficher une carte avec une paire de mots (`BigCard`).

(...)

## Conclusion

Ce code illustre une application Flutter simple et structurée :

- **Gestion d'état** avec le package `provider` (`MyAppState`), permettant d'actualiser dynamiquement l'interface.
- **Navigation responsive** via `NavigationRail` et `LayoutBuilder`, rendant l'application adaptable à différentes tailles d'écran.
- **Utilisation de widgets personnalisés** comme `BigCard` pour réutiliser du code et assurer une apparence homogène.

L'application permet ainsi de générer des paires de mots aléatoires, d'ajouter ou de retirer des favoris et de naviguer aisément entre la page de génération et celle des favoris.

---
