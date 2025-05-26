# Système de Gestion d'Événements - GestionEvent

## Description du projet

GestionEvent est une application de gestion d'événements complète permettant d'organiser, suivre et gérer différents types d'événements tels que des conférences et concerts. Cette application repose sur une architecture REST et implémente plusieurs patterns de conception, notamment Observer, Singleton et DTO.

## Table des matières

1. [Fonctionnalités](#fonctionnalités)
2. [Architecture technique](#architecture-technique)
3. [Structure du projet](#structure-du-projet)
4. [Prérequis](#prérequis)
5. [Installation et démarrage](#installation-et-démarrage)
6. [Documentation API](#documentation-api)
7. [Modèle de données](#modèle-de-données)
8. [Technologies utilisées](#technologies-utilisées)
9. [Patterns de conception](#patterns-de-conception)
10. [Tests](#tests)
11. [Auteur](#auteur)

## Fonctionnalités

L'application permet de:

### Gestion des Événements:
- Créer deux types d'événements: conférences et concerts
- Consulter, modifier et supprimer des événements
- Annuler des événements avec notification automatique aux participants
- Rechercher des événements par lieu
- Lister les événements disponibles (non complets et non annulés)

### Gestion des Participants:
- Inscrire des participants ordinaires et des organisateurs
- Consulter, modifier et supprimer des profils de participants
- Rechercher des participants par nom
- Assigner des organisateurs à des événements

### Gestion des Inscriptions:
- Inscrire et désinscrire des participants aux événements
- Limiter les inscriptions selon la capacité maximale de l'événement
- Notifier les participants lors de modifications d'événements

### Notifications:
- Système de notification asynchrone des participants
- Support pour notifications par email (configuré pour le développement)
- Notifications lors d'annulation ou modification d'événements

## Architecture technique

L'application est structurée selon le modèle MVC (Modèle-Vue-Contrôleur):

- **Modèle**: Entités JPA représentant les données de l'application
- **Vue**: API REST exposée via des contrôleurs Spring
- **Contrôleur**: Logique métier implémentée dans les services

L'architecture respecte les principes de séparation des responsabilités et d'injection de dépendances.

## Structure du projet

```
/src
  /main
    /java/com/project/POO
      /config        - Configuration Spring Boot et Swagger
      /controller    - Contrôleurs REST
      /dto           - Objets de transfert de données
      /exception     - Exceptions personnalisées et gestionnaire global
      /model         - Entités JPA
      /observer      - Interfaces pour le pattern Observer
      /repository    - Repositories JPA
      /service       - Services métier
      /utils         - Classes utilitaires
    /resources
      application.properties - Configuration de l'application
  /test
    /java/com/project/POO  - Tests unitaires et d'intégration
```

## Prérequis

- JDK 21 ou supérieur
- Maven 3.9.9 ou supérieur
- MySQL 8.0 ou supérieur
- Un IDE Java (IntelliJ IDEA, Eclipse, VS Code)

## Installation et démarrage

1. **Cloner le dépôt:**
   ```bash
   git clone https://github.com/Freddy-Gitty/GestionEvent.git
   cd gestionevent
   ```

2. **Configurer la base de données:**

   Créez une base de données MySQL nommée `eventbd`.

   Modifiez les paramètres de connexion dans `src/main/resources/application.properties`:
   ```properties
   spring.datasource.url=jdbc:mysql://localhost:3306/eventbd?useSSL=false&serverTimezone=UTC
   spring.datasource.username=votre_utilisateur
   spring.datasource.password=votre_mot_de_passe
   ```

3. **Compiler et lancer l'application:**
   ```bash
   # Avec Maven
   ./mvnw spring-boot:run
   
   # Sous Windows
   mvnw.cmd spring-boot:run
   ```

4. **Accéder à l'application:**

   L'application sera disponible à l'adresse: `http://localhost:8080`

   L'interface Swagger sera disponible à: `http://localhost:8080/swagger-ui.html`

## Documentation API

L'API REST est entièrement documentée avec Swagger/OpenAPI. Pour explorer et tester les endpoints:

1. Lancez l'application
2. Ouvrez votre navigateur à l'adresse: `http://localhost:8080/swagger-ui.html`

### Principaux endpoints:

#### Événements
- `GET /api/evenements` - Liste tous les événements
- `GET /api/evenements/{id}` - Récupère un événement par ID
- `POST /api/evenements/conferences` - Crée une nouvelle conférence
- `POST /api/evenements/concerts` - Crée un nouveau concert
- `PUT /api/evenements/{id}` - Met à jour un événement
- `DELETE /api/evenements/{id}` - Supprime un événement
- `PUT /api/evenements/{id}/annuler` - Annule un événement
- `GET /api/evenements/recherche?lieu={lieu}` - Recherche d'événements par lieu
- `GET /api/evenements/disponibles` - Liste les événements disponibles

#### Participants
- `GET /api/participants` - Liste tous les participants
- `GET /api/participants/{id}` - Récupère un participant par ID
- `POST /api/participants` - Crée un nouveau participant
- `POST /api/participants/organisateurs` - Crée un nouvel organisateur
- `PUT /api/participants/{id}` - Met à jour un participant
- `DELETE /api/participants/{id}` - Supprime un participant
- `GET /api/participants/recherche?nom={nom}` - Recherche de participants par nom

#### Inscriptions
- `POST /api/evenements/{evenementId}/participants/{participantId}` - Inscrit un participant à un événement
- `DELETE /api/evenements/{evenementId}/participants/{participantId}` - Désinscrit un participant d'un événement

## Modèle de données

### Classe Evenement (abstraite)
- **id**: Identifiant unique (UUID)
- **nom**: Nom de l'événement
- **date**: Date et heure de l'événement
- **lieu**: Lieu de l'événement
- **capaciteMax**: Capacité maximale
- **annule**: État d'annulation
- **participants**: Liste des participants inscrits
- **organisateur**: Organisateur de l'événement

### Classes dérivées
- **Conference**: Inclut thème et intervenants
- **Concert**: Inclut artiste et genre musical

### Classe Participant
- **id**: Identifiant unique (UUID)
- **nom**: Nom du participant
- **email**: Email du participant
- **evenementsInscrits**: Liste des événements auxquels le participant est inscrit

### Classe Organisateur (dérive de Participant)
- **evenementsOrganises**: Liste des événements organisés

## Technologies utilisées

### Backend
- **Spring Boot 3.4.5**: Framework principal
- **Spring Data JPA**: Persistance des données
- **Spring MVC**: Contrôleurs REST
- **Spring Mail**: Support pour l'envoi d'emails
- **Lombok**: Réduction du code boilerplate
- **Swagger/OpenAPI**: Documentation de l'API
- **JUnit 5**: Tests unitaires et d'intégration
- **Mockito**: Mocking pour les tests

### Base de données
- **MySQL 8**: Base de données de production
- **H2 Database**: Base de données en mémoire pour les tests

### Outils de build
- **Maven**: Gestion des dépendances et build
- **JaCoCo**: Couverture de tests

## Patterns de conception

### Pattern Observer
Implémenté pour les notifications d'événements. Quand un événement est modifié ou annulé, tous les participants inscrits sont automatiquement notifiés.

```java
// Interfaces
public interface EvenementObservable {
    void subscribe(ParticipantObserver observer);
    void unsubscribe(ParticipantObserver observer);
    void notifyObservers(String message);
}

public interface ParticipantObserver {
    void update(String message);
}
```

### Pattern Singleton
Implémenté pour la gestion centralisée des événements.

```java
public class GestionEvenements {
    private static GestionEvenements instance;
    
    public static synchronized GestionEvenements getInstance() {
        if (instance == null) {
            instance = new GestionEvenements();
        }
        return instance;
    }
}
```

### Pattern DTO (Data Transfer Object)
Utilisé pour séparer la représentation API des entités internes.

```java
// DTOs
public class EvenementDto { /* ... */ }
public class ParticipantDto { /* ... */ }

// Conversions
private EvenementDto convertToDto(Evenement evenement) { /* ... */ }
private Evenement convertToEntity(EvenementDto dto) { /* ... */ }
```

## Tests

L'application est couverte par une suite complète de tests unitaires et d'intégration.

- Tests unitaires pour les services, repositories et utilitaires
- Tests d'intégration pour les contrôleurs REST
- Tests pour les patterns de conception implémentés

Pour exécuter les tests:

```bash
./mvnw test
```

Pour générer un rapport de couverture de code:

```bash
./mvnw jacoco:report
```

Le rapport sera généré dans `target/site/jacoco/index.html`.

## Auteur

Ce projet a été développé par **ELA FOE FREDERIC THEOPHILE** dans le cadre d'un apprentissage avancé de de la programmation orientée objet et de Java/Spring Boot.

---


va