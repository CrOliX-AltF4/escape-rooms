# Synthèse TP Spring Core : IoC, DI & Profiles

Ce document résume les concepts clés mis en pratique lors de l'exercice **Escape Rooms**.

## 1. Inversion de Contrôle (IoC)
L'IoC consiste à déléguer la création et la gestion du cycle de vie des objets (les "Beans") au framework Spring plutôt que de les instancier manuellement avec `new`.

### Annotations de détection
- `@Component` : Marque une classe comme composant géré par Spring.
- `@Service` : Spécialisation de `@Component` pour la couche métier (BLL).

## 2. Injection de Dépendances (DI)
La DI est le processus par lequel Spring injecte les dépendances requises dans un Bean au moment de sa création.

### Modes d'injection utilisés
- **Par constructeur** (ex: `EscapeRoom2Controller`) : Recommandé pour les dépendances obligatoires.
- **Par setter** (ex: `TreasureRoomController`) : Utile pour les dépendances optionnelles ou la ré-injection.
- **Annotation `@Autowired`** : Indique à Spring où l'injection doit avoir lieu.

## 3. Gestion des Profils (`@Profile`)
Les profils permettent de conditionner l'existence d'un Bean dans le contexte Spring. 

### Fonctionnement dans le TP
1. **Déclaration** : Les services sont annotés avec `@Profile("passage")`, `@Profile("trap")`, ou `@Profile("treasure")`.
2. **Activation** : Dans `src/main/resources/application.properties`, on définit le profil actif :
   ```properties
   spring.profiles.active=passage,treasure
   ```
3. **Résultat** : Spring n'instancie que les classes dont le profil correspond à la configuration active, résolvant ainsi les ambiguïtés entre plusieurs implémentations d'une même interface.

## 4. Résumé des annotations appliquées

| Classe | Annotation Spring | Profil | Rôle |
| :--- | :--- | :--- | :--- |
| `Room1Service` | `@Service` | `passage` | Implémentation gagnante Salle 2 |
| `Room2Service` | `@Service` | `trap` | Implémentation perdante Salle 2 |
| `Treasure1Service` | `@Service` | `trap` | Implémentation piège Trésor |
| `Treasure2Service` | `@Service` | `treasure` | Implémentation gagnante Trésor |
| `EscapeRoom1Controller` | `@Component` | - | Contrôleur Salle 1 |
| `EscapeRoom2Controller` | `@Component("room2")` | - | Contrôleur Salle 2 (Injection par constructeur) |
| `TreasureRoomController` | `@Component` | - | Contrôleur Trésor (Injection par setter) |
