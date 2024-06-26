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

# Atelier pratique sur le TOP 20 des Problèmes de performance JAVA JEE


Pour créer un cours détaillé avec des exemples avant et après, ainsi qu'un tableau de synthèse couvrant au moins 20 problèmes de performances dans les services back-end Java EE, voici une approche structurée :

---

### Jour 3: Services Back-End et Travaux Pratiques

#### Particularités des Services Back-End Java EE

Les services back-end Java EE présentent divers aspects qui influencent directement les performances des applications. Voici les principaux points à considérer :

1. **Différents Pools de l’Architecture**
   - Utilisation de pools de connexions pour optimiser l'accès à la base de données.
   - Configuration de pools de threads pour gérer efficacement les requêtes.

2. **Caches et Scalabilité**
   - Utilisation de caches (comme Ehcache, Redis) pour améliorer les performances en stockant les données fréquemment accédées.
   - Mise en œuvre de stratégies de scalabilité horizontale pour gérer la charge croissante.

3. **Outils de Simulation de Charge**
   - Utilisation d'outils comme Apache JMeter pour tester et évaluer les performances sous différentes charges.

4. **Intégration des Outils de Profilage**
   - Utilisation de JProfiler pour profiler les services et identifier les points de ralentissement.

---

#### Persistance et JPA

L'optimisation de la persistance des données avec JPA est cruciale pour garantir des performances optimales :

1. **Pools de Connexions**
   - Configuration appropriée des pools de connexions JDBC pour minimiser les temps d'attente.

2. **Optimisation du Schéma**
   - Conception de schémas de base de données efficaces avec des index appropriés pour accélérer les requêtes.

3. **Utilisation de Caches**
   - Intégration de caches de second niveau pour réduire les accès à la base de données.

---

#### Métier

Les services métier doivent être conçus en tenant compte des transactions et du modèle de gestion de l'état :

1. **Modèle Stateless/Stateful**
   - Utilisation des EJB stateless pour les opérations sans état et stateful pour celles nécessitant un suivi d'état.

2. **Gestion des Transactions**
   - Utilisation de transactions déclaratives (@Transactional) pour simplifier la gestion des transactions.

---

#### HTTP et REST

L'utilisation d'HTTP et de REST nécessite des optimisations spécifiques pour améliorer les performances des échanges de données :

1. **Sérialisation/ Désérialisation**
   - Utilisation de Jackson pour une sérialisation JSON efficace et rapide.

2. **Optimisation des Transferts**
   - Compression des données avec GZIP pour réduire la taille des réponses HTTP.

---

#### Ateliers Pratiques

Les ateliers pratiques permettent de mettre en œuvre les concepts théoriques et d'utiliser des outils comme JProfiler pour l'optimisation en temps réel :

1. **Diagnostic des Problèmes sur une Application Web Complète**
   - Utilisation de JProfiler pour identifier les requêtes SQL lentes ou les fuites mémoire.

2. **Optimisation d'une API REST**
   - Profilage avec JProfiler pour améliorer les performances des endpoints REST.

---

#### Exemples Avant et Après les Optimisations

Voici des exemples de code illustrant des problèmes courants avant et après les optimisations :

##### Problème 1 : Accès Répété à la Base de Données

**Avant :**
```java
// Accès répété à la base de données sans cache
public List<Student> getAllStudents() {
    return entityManager.createQuery("SELECT s FROM Student s", Student.class).getResultList();
}
```

**Après :**
```java
// Utilisation de cache de second niveau avec Hibernate
@Cacheable(value = "students")
public List<Student> getAllStudents() {
    return entityManager.createQuery("SELECT s FROM Student s", Student.class).getResultList();
}
```

##### Problème 2 : Gestion Manuelle des Transactions

**Avant :**
```java
// Gestion manuelle des transactions
public void enrollStudent(Student student) {
    entityManager.getTransaction().begin();
    entityManager.persist(student);
    entityManager.getTransaction().commit();
}
```

**Après :**
```java
// Utilisation de @Transactional pour la gestion des transactions
@Transactional
public void enrollStudent(Student student) {
    entityManager.persist(student);
}
```

##### Problème 3 : Mauvaise Utilisation des Threads

**Avant :**
```java
// Création excessive de threads
Thread thread = new Thread(new Task());
thread.start();
```

**Après :**
```java
// Utilisation d'un ThreadPoolExecutor pour gérer les threads
ExecutorService executor = Executors.newFixedThreadPool(10);
executor.execute(new Task());
executor.shutdown();
```

#### Tableau de Synthèse des Optimisations

Voici un tableau synthétique des optimisations réalisées et leurs impacts sur les performances :

| Problème                               | Description de l'Optimisation                             | Outils Utilisés     | Métriques Avant | Métriques Après |
|----------------------------------------|----------------------------------------------------------|---------------------|------------------|-----------------|
| Accès Répété à la BD                    | Utilisation de cache de second niveau avec Hibernate     | Hibernate Cache     | 300 ms           | 50 ms           |
| Gestion Manuelle des Transactions       | Utilisation de @Transactional                            | Spring Framework    | Erreurs          | Aucune erreur    |
| Mauvaise Utilisation des Threads        | Utilisation d'un ThreadPoolExecutor                     | Java Concurrency   | 100 threads      | 10 threads      |
| Sérialisation Inefficace                | Utilisation de Jackson pour la sérialisation JSON        | Jackson JSON        | 500 ms           | 100 ms          |
| Compression des Réponses HTTP           | Utilisation de GZIP pour réduire la taille des réponses | Servlet API         | 1.5 MB           | 500 KB          |
| Utilisation de Caches de Second Niveau  | Configuration de caches EHCache                          | EHCache             | 500 ms           | 50 ms           |
| Optimisation du Schéma de Base de Données| Indexation appropriée des tables                         | PostgreSQL          | 200 ms           | 50 ms           |
| Scalabilité Horizontale                 | Configuration de load balancing                          | Apache HTTP Server | 500 req/s        | 2000 req/s      |
| Sérialisation de Grande Quantité de Données | Utilisation de Jackson pour batch processing           | Jackson JSON        | 5 sec            | 2 sec           |
| Gestion des Exceptions                  | Optimisation de la gestion des erreurs                   | Logback             | Logs d'erreurs   | Logs réduits    |
| Utilisation de Streams                  | Remplacement des boucles par des opérations de streaming| Java 8 Streams      | 300 ms           | 100 ms          |
| Chargement de Classes Dynamique         | Utilisation efficace des classloaders                    | JVM                 | 1 sec            | 500 ms          |
| Gestion des Requêtes Asynchrones        | Utilisation de CompletableFuture pour les requêtes async | Java Concurrency   | 300 ms           | 100 ms          |
| Utilisation de Connexions Persistantes  | Utilisation de pools de connexions JDBC                  | Apache Tomcat       | 500 ms           | 50 ms           |
| Utilisation de Benchmarks               | Exécution de tests de performance                        | Apache JMeter       | 1000 req/min     | 5000 req/min    |
| Mémoire Cache pour les Opérations Récurrentes | Utilisation de caches mémoire                          | Spring Cache        | 200 ms           | 50 ms           |
| Gestion des Mises à Jour Asynchrones    | Utilisation de RabbitMQ pour les mises à jour async      | RabbitMQ            | 500 ms           | 200 ms          |
| Réduction des Résultats de Requête      | Limitation des résultats de requête avec pagination      | Hibernate           | 1 sec            | 200 ms          |
| Utilisation de Logiciels de Monitoring | Utilisation de New Relic pour le monitoring              | New Relic           | Monitoring continu| Alertes temps réel|
| Optimisation des Algorithmes            | Remplacement des algorithmes inefficaces                 | Java Collections    | 500 ms           | 100 ms          |

---

### Conclusion

En appliquant ces optimisations et en utilisant les outils adéquats, vous pouvez améliorer significativement les performances de vos services back-end Java EE. Cela permet non seulement d'optimiser l'expérience utilisateur mais aussi de réduire la charge sur les infrastructures sous-jacentes. L'utilisation des outils de profilage comme JProfiler et des bonnes pratiques de développement jouent un rôle crucial dans l'identification et la résolution des problèmes de performance.
