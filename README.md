# Projet-ALP : Système de Surveillance de Capteur
Ce projet simule un système de surveillance de **Capteur** (capteur) avec des observateurs qui réagissent aux changements des données du capteur. Le système est conçu pour démontrer le **modèle de conception Observer**, les **mises à jour asynchrones** et les **stratégies de test**.

## Fonctionnalités

- **Capteur (Sensor)** : Le composant principal qui produit les données du capteur.
- **Modèle Observer** : Mise en œuvre d'un système d'observateurs qui réagissent aux mises à jour des données du capteur.
- **Observateurs Asynchrones** : Utilisation de tâches asynchrones pour simuler les mises à jour en temps réel des capteurs.
- **Tâches Planifiées** : Le capteur se met à jour périodiquement en utilisant un exécutant planifié.
- **Observateurs** : Plusieurs observateurs (`Canal`, `Afficheur`) consomment et affichent les données.

## Concepts Abordés

### 1. **Les Stratégies**

Dans ce projet, nous appliquons le **modèle de conception Strategy** pour gérer différents types d'observateurs et la manière dont ils traitent les données. Les observateurs peuvent être mis à jour de manière asynchrone, en utilisant des délais aléatoires pour simuler un comportement en temps réel. Le **Strategy** permet une extensibilité future pour ajouter de nouveaux comportements d'observateurs sans changer la logique principale.

#### Exemple d'application de la stratégie :

Les observateurs (`Canal`) peuvent implémenter différentes stratégies de mise à jour. Par exemple :
- **Récupération des données en temps réel** : Les observateurs peuvent récupérer immédiatement les données après une mise à jour.
- **Récupération des données avec retard** : Les observateurs récupèrent les données après un certain délai, simulant un flux de données lent.

Ce modèle permet une grande flexibilité pour ajouter de nouvelles stratégies concernant la manière dont les observateurs interagissent avec le capteur.

### 2. **Les Oracles**

Un **oracle** dans ce projet fait référence à un mécanisme permettant de vérifier que les observateurs reçoivent correctement les données mises à jour du capteur.

Dans notre cas, un oracle pourrait être :
- **Une vérification lisible par un humain** : Simplement imprimer les valeurs reçues par chaque observateur (`Canal` et `Afficheur`), et vérifier manuellement si les valeurs attendues correspondent aux valeurs imprimées.

## Diagrammes UML

### Diagramme de Classes
![Diagramme de Classes](./path_to_your_image/Class_Diagram.png)

### Diagramme de Séquence
![Diagramme de Séquence](./path_to_your_image/Sequence_Diagram.png)

## Tests

Une suite de tests unitaires a été mise en place pour vérifier la robustesse et la cohérence du système. Les tests utilisent JUnit et Mockito, et couvrent plusieurs scénarios pour valider le comportement du système.

### Scénarios Testés

1. **Attach Observer**  
   Vérifie que les observateurs sont correctement attachés au capteur et qu'une exception est levée si un observateur est déjà attaché.

2. **Tick et Notification des Observateurs**  
   Simule une mise à jour du capteur (`tick`) et vérifie que tous les observateurs sont notifiés correctement.

3. **Mise à Jour Asynchrone**  
   Teste la fonctionnalité de mise à jour asynchrone des observateurs.

4. **Fermeture (Shutdown)**  
   Vérifie que les ressources liées aux observateurs (par exemple, les exécuteurs planifiés) sont correctement libérées après fermeture.

5. **Attach Multiple Observers**  
   Teste le comportement lorsque plusieurs observateurs sont attachés au capteur.

6. **Shutdown Global**  
   Simule l'arrêt complet de l'application et vérifie que tous les composants sont correctement arrêtés.

### Exemple de Code de Test

```java
@Test
public void testAttachObserver() throws NoSuchFieldException, IllegalAccessException {
    capteur.attach(canal1);
    
    Field observersField = CapteurImpl.class.getDeclaredField("observers");
    observersField.setAccessible(true);
    @SuppressWarnings("unchecked")
    List<Canal> observers = (List<Canal>) observersField.get(capteur);
    
    assertEquals(1, observers.size(), "Observer should be attached");
}
