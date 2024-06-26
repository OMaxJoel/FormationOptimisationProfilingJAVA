### Jour 3: Services Back-End et Travaux Pratiques

Les services back-end JavaEE jouent un rôle essentiel dans la logique métier, la gestion des données et la fourniture d'API robustes. Optimiser ces services est crucial pour assurer des performances élevées, une scalabilité efficace et une utilisation efficace des ressources système.


#### Particularités des Services Back-End JavaEE

Les services back-end JavaEE se distinguent par leur architecture et leur capacité à supporter des charges de travail importantes.

##### Différents Pools de l’Architecture, Caches, Scalabilité

**Pools de Connexions :** Les pools de connexions permettent de gérer efficacement les accès à la base de données. Une configuration appropriée est essentielle pour éviter les temps d'attente excessifs.

**Exemple de configuration sans optimisation :**
```java
DataSource dataSource = ...; // Initialisation de la source de données
Connection connection = dataSource.getConnection(); // Obtention d'une connexion
// Utilisation de la connexion
```

**Bonnes pratiques :** Utilisation de HikariCP pour configurer les pools de connexions avec des paramètres comme `maximumPoolSize`, `minimumIdle`, et `connectionTimeout` pour optimiser les performances et la scalabilité.

##### Outils de Simulation de Charge, Intégration des Serveurs JProfiler

**Simulation de Charge :** Tester avec des outils comme JMeter ou Apache Bench aide à optimiser la gestion des requêtes et des réponses.

**Intégration des Serveurs JProfiler :** Utilisation de JProfiler pour profiler et optimiser les performances des services back-end.

#### Persistance et JPA

La persistance des données avec Java Persistence API (JPA) est cruciale pour les services back-end.

##### Pools de Connexions, Optimisation du Schéma, Caches

**Optimisation du Schéma :** Un schéma bien conçu avec des index appropriés améliore la performance des requêtes.

**Exemple de requête JPA sans optimisation :**
```java
// Exemple avant : Requête JPA simple
EntityManager em = ...; // Initialisation de l'EntityManager
Query query = em.createQuery("SELECT e FROM Employee e WHERE e.department = :dept");
query.setParameter("dept", department);
List<Employee> employees = query.getResultList();
```

**Bonnes pratiques :** Utilisation de `@NamedQuery` pour précompiler les requêtes JPA et utilisation des caches de second niveau pour éviter les requêtes répétitives.

#### Métier

Les services métier doivent être efficaces et réactifs.

##### Modèle Stateless/Stateful, Transactions

**Modèle Stateless/Stateful :** Utilisation du modèle stateless pour minimiser la consommation de ressources.

**Exemple de bean stateful :**
```java
// Exemple avant : Bean stateful
@Stateful
public class ShoppingCartBean implements ShoppingCart {
    private List<Item> items = new ArrayList<>();

    public void addItem(Item item) {
        items.add(item);
    }

    public List<Item> getItems() {
        return items;
    }
}
```

**Bonnes pratiques :** Utilisation du modèle stateless autant que possible pour maximiser la scalabilité.

#### HTTP et REST

Les services back-end exposent souvent des API REST pour interagir avec les clients.

##### Sérialisation/Désérialisation, Optimisation des Transferts

**Sérialisation/Désérialisation :** Utilisation de Jackson pour optimiser la sérialisation JSON.

**Exemple de désérialisation inefficace :**
```java
// Exemple avant : Désérialisation JSON
ObjectMapper mapper = new ObjectMapper();
Item item = mapper.readValue(jsonString, Item.class);
```

**Bonnes pratiques :** Configuration de Jackson pour ignorer les propriétés inconnues et contrôler l'inclusion des propriétés dans la sérialisation.

#### Ateliers Pratiques

Les travaux pratiques permettent d'appliquer les concepts théoriques et d'identifier les problèmes de performance spécifiques.

##### Diagnostic de Problèmes sur une Application Web Complète, sur une API Rest

**Monitoring et Profiling :** Utilisation de NetBeans Profiler ou JProfiler pour monitorer les performances et optimiser les services back-end.

**Exemple d'utilisation de NetBeans Profiler :**
```java
// Exemple après : Utilisation de NetBeans Profiler
public class MyApplication extends Application {
    @Override
    public Set<Class<?>> getClasses() {
        return new HashSet<>(Arrays.asList(MyResource.class));
    }

    public static void main(String[] args) {
        // Lancer l'application avec profiling
        Profiler.start();
        // Code de démarrage de l'application
        Profiler.stop();
    }
}
```


## Code Avant et Après


#### Particularités des Services Back-End JavaEE

##### Différents Pools de l’Architecture, Caches, Scalabilité

**Exemple avant :** Configuration basique des pools de connexions sans optimisation spécifique.

```java
// Exemple avant : Configuration basique des pools de connexions
DataSource dataSource = ...; // Initialisation de la source de données
Connection connection = dataSource.getConnection(); // Obtention d'une connexion
// Utilisation de la connexion
```

**Exemple après :** Configuration personnalisée des pools de connexions avec gestion des timeouts et ajustement des paramètres de pool.

```java
// Exemple après : Configuration personnalisée des pools de connexions
HikariConfig config = new HikariConfig();
config.setJdbcUrl("jdbc:mysql://localhost:3306/mydatabase");
config.setUsername("username");
config.setPassword("password");
config.setMaximumPoolSize(20);
config.setMinimumIdle(5);
config.setConnectionTimeout(30000); // Timeout en millisecondes

DataSource dataSource = new HikariDataSource(config);
Connection connection = dataSource.getConnection();
// Utilisation de la connexion
```

##### Outils de Simulation de Charge, Intégration des Serveurs JProfiler

**Exemple avant :** Absence de simulation de charge et de profiling avancé.

```java
// Exemple avant : Absence de simulation de charge
public void handleRequest() {
    // Logique métier
}
```

**Exemple après :** Utilisation de JProfiler pour simuler une charge de travail et profiler les performances.

```java
// Exemple après : Utilisation de JProfiler pour simuler une charge de travail
public void handleRequest() {
    // Logique métier
    Profiler.start("handleRequest"); // Début du profiling
    // Code à profiler
    Profiler.stop(); // Fin du profiling
}
```

#### Persistance et JPA

##### Pools de Connexions, Optimisation du Schéma, Caches

**Exemple avant :** Requête JPA sans optimisation du schéma.

```java
// Exemple avant : Requête JPA simple
EntityManager em = ...; // Initialisation de l'EntityManager
Query query = em.createQuery("SELECT e FROM Employee e WHERE e.department = :dept");
query.setParameter("dept", department);
List<Employee> employees = query.getResultList();
```

**Exemple après :** Optimisation de la requête JPA avec utilisation de cache de second niveau.

```java
// Exemple après : Optimisation de la requête JPA avec cache de second niveau
@Cacheable(true)
public List<Employee> findEmployeesByDepartment(String department) {
    EntityManager em = ...; // Initialisation de l'EntityManager
    Query query = em.createQuery("SELECT e FROM Employee e WHERE e.department = :dept");
    query.setParameter("dept", department);
    return query.getResultList();
}
```

#### Métier

##### Modèle Stateless/Stateful, Transactions

**Exemple avant :** Utilisation de beans stateful sans nécessité.

```java
// Exemple avant : Bean stateful pour la gestion des états
@Stateful
public class ShoppingCartBean implements ShoppingCart {
    private List<Item> items = new ArrayList<>();

    public void addItem(Item item) {
        items.add(item);
    }

    public List<Item> getItems() {
        return items;
    }
}
```

**Exemple après :** Utilisation appropriée de beans stateless pour les opérations sans état.

```java
// Exemple après : Utilisation de beans stateless pour les opérations sans état
@Stateless
public class InventoryBean implements Inventory {
    public void updateStock(Item item, int quantity) {
        // Logique de mise à jour du stock
    }

    public List<Item> getAvailableItems() {
        // Récupération des articles disponibles
        return Collections.emptyList();
    }
}
```

#### HTTP et REST

##### Sérialisation/Désérialisation, Optimisation des Transferts

**Exemple avant :** Utilisation inefficace de la désérialisation JSON.

```java
// Exemple avant : Désérialisation JSON inefficace
ObjectMapper mapper = new ObjectMapper();
Item item = mapper.readValue(jsonString, Item.class);
```

**Exemple après :** Utilisation de bibliothèques optimisées pour la désérialisation JSON.

```java
// Exemple après : Désérialisation JSON optimisée avec Jackson
ObjectMapper mapper = new ObjectMapper();
mapper.configure(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES, false);
Item item = mapper.readValue(jsonString, Item.class);
```

#### Ateliers Pratiques

##### Diagnostic de Problèmes sur une Application Web Complète, sur une API Rest

**Exemple avant :** Déploiement sans outils de monitoring avancés.

```java
// Exemple avant : Déploiement sans monitoring avancé
public class MyApplication extends Application {
    @Override
    public Set<Class<?>> getClasses() {
        return new HashSet<>(Arrays.asList(MyResource.class));
    }
}
```

**Exemple après :** Utilisation de NetBeans Profiler pour monitorer et analyser les performances en temps réel.

```java
// Exemple après : Utilisation de NetBeans Profiler pour monitorer les performances
public class MyApplication extends Application {
    @Override
    public Set<Class<?>> getClasses() {
        return new HashSet<>(Arrays.asList(MyResource.class));
    }

    public static void main(String[] args) {
        // Lancer l'application avec profiling
        Profiler.start();
        // Code de démarrage de l'application
        Profiler.stop();
    }
}
```

---

Ces exemples montrent comment chaque concept abordé dans le Jour 3 peut être implémenté en Java, en mettant l'accent sur les bonnes pratiques et l'optimisation des performances.

---

Ce cours complet sur les Services Back-End JavaEE et les techniques d'optimisation fournit une compréhension approfondie des bonnes pratiques et des outils avancés nécessaires pour optimiser les performances des applications Java.
