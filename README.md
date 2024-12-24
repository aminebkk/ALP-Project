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

#### Exemple d'Oracle :
Par exemple, si un observateur est censé récupérer la valeur mise à jour du capteur après un "tick", l'oracle pourrait garantir que la valeur retournée par l'observateur est cohérente avec la valeur du capteur à ce moment-là.

```java
public class Afficheur implements ObserverDeCapteur {
    private final String name;
    private final Canal canal;

    public Afficheur(String name, Canal canal) {
        this.name = name;
        this.canal = canal;
    }

    @Override
    public void update() {
        Integer value = canal.get();
        if (value != null) {
            System.out.println(name + " a reçu la valeur : " + value);
            // Oracle check : La valeur imprimée doit correspondre à la valeur mise à jour du capteur.
        }
    }
}
```
L'oracle ici consisterait à vérifier que les valeurs imprimées par Afficheur correspondent aux valeurs attendues, basées sur les données du Capteur.
