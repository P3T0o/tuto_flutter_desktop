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

# 🏗️ Explication complète du projet Flutter (macOS)

Ce document explique en détail l’architecture du projet, les fichiers et le code source. Il est conçu pour être compréhensible même par un débutant.

---

## 🔹 Introduction

Ce projet est une application Flutter pour macOS qui génère des paires de mots aléatoires et permet de les ajouter aux favoris. Il utilise **Provider** pour la gestion d'état et une navigation avec **NavigationRail**.

---

## 🔹 `main.dart`

### **📌 Rôle**
C'est le point d'entrée de l'application.

### **💡 Explication**
```dart
import 'package:flutter/material.dart';
import 'package:provider/provider.dart';
import 'app.dart';
import 'state/app_state.dart';

void main() {
  runApp(
    ChangeNotifierProvider(
      create: (context) => MyAppState(),
      child: MyApp(),
    ),
  );
}
```
Démarre l'application :
```dart
runApp(MyApp())
```
Fournit l'état global (MyAppState) à l'application :
```dart
ChangeNotifierProvider()
```
---
## 🔹 `app.dart`

### **📌 Rôle**
Définit l'application et son thème.

### **💡 Explication**
```dart
import 'package:flutter/material.dart';
import 'screens/home_page.dart';

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Tuto Flutter',
      theme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepOrange),
      ),
      home: MyHomePage(),
    );
  }
}
```
Structure de l'application flutter :
```dart
MaterialApp()
```
Définit les couleurs et styles :
```dart
ThemeData()
```
Affiche l'écran principal :
```dart
home: MyHomePage()
```
---
## 🔹 `state/app_state.dart`

### **📌 Rôle**
Gère l'état de l'application (mot aléatoire et favoris).

### **💡 Explication**
```dart
import 'package:english_words/english_words.dart';
import 'package:flutter/material.dart';

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
Génère un mot aléatoire :
```dart
WordPair.random()
```
Met à jour l'interface quand les données changent :
```dart
notifyListeners()
```
Ajoute/enlève un mot des favoris :
```dart
toggleFavorite()
```
---
## 🔹 `screens/home_page.dart`

### **📌 Rôle**
Affiche la navigation et permet de basculer entre les pages.

### **💡 Explication**
```dart
import 'package:flutter/material.dart';
import 'generator_page.dart';
import 'favorites_page.dart';

class MyHomePage extends StatefulWidget {
  @override
  State<MyHomePage> createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  var selectedIndex = 0;

```
Permet de changer de page dynamiquement :
```dart
StatefulWidget
```
Stocke l’index de la page sélectionnée :
```dart
selectedIndex
```
---
```dart
  @override
Widget build(BuildContext context) {
  Widget page;
  switch (selectedIndex) {
    case 0:
      page = GeneratorPage();
      break;
    case 1:
      page = FavoritesPage();
      break;
  }
```
Affiche la bonne page en fonction du menu :
```dart
switch(selectedIndex)
```
---
```dart
    return Scaffold(
body: Row(
children: [
SafeArea(
child: NavigationRail(
extended: constraints.maxWidth >= 600,
destinations: const [
NavigationRailDestination(
icon: Icon(Icons.home),
label: Text('Home'),
),
NavigationRailDestination(
icon: Icon(Icons.favorite),
label: Text('Favorites'),
),
],
selectedIndex: selectedIndex,
onDestinationSelected: (value) {
setState(() {
selectedIndex = value;
});
},
),
),
Expanded(
child: page,
),
],
),
);

```
Barre de navigation latérale :
```dart
NavigationRail()
```
Change la page quand l’utilisateur clique :
```dart
onDestinationSelected
```
---
## 🔹 `screens/generator_page.dart`

### **📌 Rôle**
Affiche un mot aléatoire et permet de l’ajouter aux favoris.

### **💡 Explication**
```dart
import 'package:flutter/material.dart';
import 'package:provider/provider.dart';
import '../state/app_state.dart';
import '../widgets/big_card.dart';

class GeneratorPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    var appState = context.watch<MyAppState>();
    var pair = appState.current;
```
Accède aux données :
```dart
context.watch<MyAppState>()
```
---
```dart
    return Center(
child: Column(
mainAxisAlignment: MainAxisAlignment.center,
children: [
BigCard(pair: pair),
SizedBox(height: 10),
Row(
mainAxisSize: MainAxisSize.min,
children: [
ElevatedButton.icon(
onPressed: () => appState.toggleFavorite(),
icon: Icon(Icons.favorite),
label: const Text('Like'),
),
ElevatedButton(
onPressed: () => appState.getNext(),
child: const Text('Next'),
),
],
),
],
),
);
}
}
```
Ajoute/enlève un mot des favoris :
```dart
Bouton "Like"
```
Génère un nouveau mot :
```dart
Bouton "Next"
```
## 🔹 `screens/favorites_page.dart`

### **📌 Rôle**
Affiche la liste des mots favoris.

### **💡 Explication**
```dart
import 'package:flutter/material.dart';
import 'package:provider/provider.dart';
import '../state/app_state.dart';

class FavoritesPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    var appState = context.watch<MyAppState>();

    if (appState.favorites.isEmpty) {
      return const Center(
        child: Text('No favorites yet.'),
      );
    }

    return ListView(
      children: [
        Padding(
          padding: const EdgeInsets.all(20),
          child: Text('You have '
              '${appState.favorites.length} favorites:'),
        ),
        for (var pair in appState.favorites)
          ListTile(
            leading: Icon(Icons.favorite),
            title: Text(pair.asLowerCase),
          ),
      ],
    );
  }
}
```
- Affiche "No favorites yet." si la liste est vide.
- Utilise une ListView pour afficher les favori
---
## 🔹 `widgets/big_card.dart`

### **📌 Rôle**
Affiche le mot aléatoire sur une carte stylisée.

### **💡 Explication**
```dart
import 'package:flutter/material.dart';
import 'package:english_words/english_words.dart';

class BigCard extends StatelessWidget {
  final WordPair pair;

  const BigCard({super.key, required this.pair});

  @override
  Widget build(BuildContext context) {
    final theme = Theme.of(context);
    final style = theme.textTheme.displayMedium!.copyWith(
      color: theme.colorScheme.onPrimary,
    );

    return Card(
      color: theme.colorScheme.primary,
      child: Padding(
        padding: const EdgeInsets.all(20),
        child: Text(
          pair.asLowerCase,
          style: style,
        ),
      ),
    );
  }
}
```
Affiche une carte avec le mot :
```dart
Card()
```
- Utilisation du theme : Adapte la couleur au thème de l’application.







