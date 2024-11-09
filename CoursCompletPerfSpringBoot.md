### **Cours Détaillé : Optimisation des Couches d’Application Spring Boot avec Bonnes Pratiques, Métriques et Utilisation de JProfiler**

#### **Introduction**

Dans le contexte des applications Spring Boot modernes, l'optimisation des performances et la gestion efficace des ressources sont des enjeux cruciaux pour garantir une expérience utilisateur fluide et une scalabilité adaptée. Que vous développiez une application de gestion de produits, de traitement de données ou un service API, l'optimisation des différentes couches de votre architecture (Controller, Service, Persistence) permet non seulement d'améliorer la vitesse d'exécution, mais aussi de réduire la consommation de ressources et de mieux gérer les pics de trafic.

Ce cours se concentre sur les **meilleures pratiques d'optimisation** pour les couches Controller, Service et Persistence dans une application Spring Boot, en intégrant des solutions telles que la mise en cache, le traitement asynchrone, la gestion des transactions, et l'optimisation des requêtes JPA. L'accent sera mis sur l'importance de l'analyse de performance avec l'outil JProfiler, qui permet de surveiller l'impact des optimisations en termes de temps d'exécution, de consommation mémoire et de threads.

---

### **Enjeux et Défis de l’Optimisation des Applications Spring Boot**

L'optimisation des applications n'est pas seulement une question de **réduire les temps de réponse**, mais aussi d'**améliorer l'extensibilité** et de rendre l'application **résiliente face aux pics de charge**. Cela implique une gestion efficace des ressources serveur, la réduction des appels inutiles à la base de données, et l'amélioration de la réactivité grâce à des stratégies adaptées.

Les **principaux enjeux** sont :

- **Amélioration des performances** : Il s'agit d'optimiser le temps de réponse des API, la gestion des transactions et l'accès aux bases de données.
- **Réduction de la charge sur le serveur** : Moins de requêtes inutiles, moins de gestion de la mémoire, plus de gestion en cache.
- **Scalabilité** : Pouvoir supporter un nombre de requêtes ou d’utilisateurs en constante augmentation sans compromettre les performances.

Les **défis** associés à cette optimisation incluent :
- L’équilibrage entre optimisation de la performance et maintenabilité du code.
- La gestion des optimisations sans causer d’effets secondaires négatifs, comme une surcharge mémoire due à une gestion excessive du cache.
- Les **problèmes de concurrence** et l'implémentation du parallélisme sans risquer de conflits ou de conditions de course.

---

### **Menaces, Risques et Opportunités de l’Optimisation**

#### **Menaces**
1. **Incohérence des données** : Les optimisations peuvent parfois entraîner des problèmes d'intégrité des données, surtout dans le cas d'une mauvaise gestion des transactions.
2. **Fuites de mémoire** : L'usage excessif de la mémoire par des caches non gérés ou des traitements parallèles mal configurés peut engendrer des fuites de mémoire.
3. **Surcharge du serveur** : Si mal implémenté, l’usage de la mise en cache ou du parallélisme peut causer des problèmes de surcharge serveur.

#### **Risques**
1. **Surcharge réseau et CPU** : Une mauvaise gestion des requêtes asynchrones ou parallèles peut entraîner une surcharge CPU, ce qui ralentit l’application.
2. **Risque de latence** : Si les mécanismes de cache ne sont pas configurés correctement, cela pourrait introduire des latences supplémentaires, voire des incohérences dans les données récupérées.
3. **Sécurité** : L'implémentation incorrecte de solutions comme la mise en cache distribuée ou le parallélisme pourrait rendre l'application vulnérable aux attaques de type cache poisoning ou à des conditions de course.

#### **Opportunités**
1. **Amélioration de la satisfaction utilisateur** : La réduction des temps de réponse grâce à des optimisations adéquates peut considérablement améliorer l’expérience utilisateur.
2. **Évolutivité** : L’optimisation des couches de l’application permet d’améliorer la scalabilité, rendant l’application apte à gérer une augmentation du nombre d’utilisateurs.
3. **Réduction des coûts** : Une gestion optimisée des ressources serveur et de la mémoire permet de diminuer les coûts liés à l’infrastructure et au cloud.

---

### **Stratégie d’Optimisation et Objectifs**

La stratégie d’optimisation se concentrera sur plusieurs aspects fondamentaux, à savoir la gestion de la **performance des API**, l’optimisation des requêtes **JPA**, le **parallélisme** des traitements, et l’**implémentation de mécanismes de cache** efficaces.

**Objectifs de l’optimisation** :
1. Réduire les temps de réponse des API (temps d'exécution inférieur à 100ms pour la majorité des appels).
2. Améliorer l’évolutivité de l'application pour supporter une croissance du trafic de 50 % par an sans dégradation des performances.
3. Implémenter des mécanismes de mise en cache robustes et distribués (Redis) pour améliorer les performances globales.

---

### **Métriques de Performance**

Les métriques de performance sont essentielles pour mesurer l’impact des optimisations. Nous allons utiliser **JProfiler** pour surveiller :

- **Temps de réponse** des API (avant et après optimisation).
- **Utilisation de la mémoire** et **consommation CPU** pour identifier les goulots d'étranglement.
- **Nombre de requêtes HTTP** traitées par seconde.
- **Temps d'exécution des transactions**.
- **Taux d'occupation des threads** (parallélisme et gestion des threads).
  
Voici un ensemble de métriques à suivre :
- **Temps moyen de réponse** : Un objectif d’un temps de réponse moyen inférieur à 100ms.
- **Utilisation mémoire** : La mémoire doit être utilisée de manière optimale, sans pics de consommation excessifs.
- **Utilisation du CPU** : Il est essentiel de ne pas saturer les ressources CPU, particulièrement lors des pics de charge.

---

### **Top 20 des Bonnes Pratiques pour l'Optimisation d'Application Spring Boot**

#### **Optimisations dans la couche Controller**:
1. **Utiliser le cache de manière judicieuse** (`@Cacheable`) pour les données statiques.
2. **Appliquer la pagination pour les réponses avec beaucoup de données** (`Pageable`).
3. **Eviter les appels API synchrones lourds** : utiliser l’**exécution asynchrone** (`@Async`).
4. **Utiliser des appels batch pour réduire la latence** lors du traitement des grandes quantités de données.

#### **Optimisations dans la couche Service**:
5. **Gestion des transactions avec `@Transactional`** pour garantir la cohérence des données.
6. **Exécution parallèle des tâches indépendantes** avec `CompletableFuture`.
7. **Effectuer un prétraitement des données pour alléger les appels en base de données**.
8. **Configurer le pool de connexions pour limiter les requêtes bloquantes**.

#### **Optimisations dans la couche Persistence**:
9. **Eviter le problème N+1 avec `JOIN FETCH`** dans les requêtes JPA.
10. **Activer le cache de second niveau** avec Hibernate pour des accès fréquents.
11. **Utiliser les index pour améliorer les requêtes sur des colonnes fréquemment interrogées**.
12. **Optimiser les requêtes avec des projections pour éviter de charger des données inutiles**.
13. **Utiliser les requêtes paramétrées pour prévenir les injections SQL**.

#### **Optimisation du cache**:
14. **Configurer Redis pour le cache distribué**.
15. **Utiliser Caffeine pour le cache local afin de réduire les appels réseau**.
16. **Mettre en place un mécanisme d'expiration des caches pour éviter la surcharge mémoire**.
17. **Gérer le cache à l’aide de TTL (Time To Live) pour éviter les données obsolètes**.

#### **Optimisation générale**:
18. **Analyser les performances avec un profiler comme JProfiler** avant et après l’optimisation.
19. **Limiter la taille des réponses HTTP en compressant les données JSON**.
20. **Minimiser les appels aux ressources externes et privilégier l'utilisation de services internes pour la récupération des données**.

---

### **Plan Détaillé des Optimisations à Implémenter**

#### **1. Optimisation de la couche Controller**
Les optimisations ici incluent la gestion de la **mise en cache**, la **pagination**, et l’utilisation de la **programmation asynchrone**. L’objectif est de minimiser le temps d’attente des utilisateurs en réduisant la latence dans la gestion des requêtes HTTP.

##### **Implémentation :**
Nous allons intégrer des mécanismes de cache pour réduire la charge sur la base de données, et ajouter une gestion asynchrone des appels API lourds pour améliorer la réactivité.

#### **2. Optimisation de la couche Service**
Ici, nous nous concentrerons sur l’optimisation des **transactions**, l’**exécution parallèle** et la **gestion efficace des tâches en arrière-plan** pour améliorer les performances des opérations métier.

##### **Implémentation :**
Nous allons ajuster les méthodes pour qu'elles utilisent des transactions **atomiques**, et paralléliser les appels indépendants via `CompletableFuture`.

#### **3. Optimisation de la couche Persistence**
Nous nous attaquerons ici aux problèmes classiques liés à l'optimisation des requêtes

, telles que le problème **N+1**, et l'usage des **index** pour optimiser les requêtes fréquentes.

##### **Implémentation :**
Les modifications incluent l'ajout d'index sur les colonnes les plus souvent recherchées et la refactorisation des requêtes pour éviter les appels répétitifs à la base de données.

---

### Conclusion

En mettant en œuvre ces optimisations à différents niveaux de l’application Spring Boot, vous pourrez significativement améliorer les performances, réduire les coûts d’infrastructure et garantir une meilleure expérience utilisateur. L’outil JProfiler, intégré à ce processus, permettra de mesurer l'impact de ces optimisations en temps réel, vous offrant une vision claire de la manière dont vos changements affectent les performances et la gestion des ressources.

### Suite du Cours : Optimisation des Couches d'Application Spring Boot avec Bonnes Pratiques, Métriques et Utilisation de JProfiler

---

### **Optimisation de la Couche Controller**

#### **Introduction**

La couche **Controller** d'une application Spring Boot est responsable de la gestion des requêtes HTTP entrantes, de la coordination avec les services et de l’envoi des réponses au client. Dans des applications à fort trafic, la gestion efficace de cette couche est cruciale pour assurer une réactivité optimale et une faible latence des réponses. Plusieurs techniques d'optimisation peuvent être appliquées pour réduire le temps de réponse et alléger la charge du serveur, notamment **la mise en cache**, **la pagination**, et **le traitement asynchrone**.

#### **Enjeux et Défis**

Les principaux défis liés à la couche Controller sont la gestion des **données volumineuses** et des **temps de réponse** élevés. Ces problèmes peuvent survenir lorsque des requêtes complexes ou des appels API externes sont effectués, entraînant des délais de réponse plus longs et une surcharge du serveur.

#### **Stratégies d’Optimisation**

1. **Mise en Cache** : Utiliser le cache pour éviter les appels répétés à la base de données ou aux services externes.
   - **Exemple avant optimisation** :
     ```java
     @RestController
     public class ProductController {
         @Autowired
         private ProductService productService;

         @GetMapping("/products/{id}")
         public Product getProduct(@PathVariable Long id) {
             return productService.getProductById(id);
         }
     }
     ```
   - **Exemple après optimisation avec cache** :
     ```java
     @RestController
     public class ProductController {
         @Autowired
         private ProductService productService;

         @Cacheable(value = "productCache", key = "#id")
         @GetMapping("/products/{id}")
         public Product getProduct(@PathVariable Long id) {
             return productService.getProductById(id);
         }
     }
     ```
   - **Impact attendu** : Réduction de la charge sur la base de données et des temps de réponse pour les appels répétés.

2. **Pagination** : Pour les API qui renvoient de grandes quantités de données, il est important de limiter le nombre de résultats retournés.
   - **Exemple avant optimisation** :
     ```java
     @GetMapping("/products")
     public List<Product> getAllProducts() {
         return productService.getAllProducts();
     }
     ```
   - **Exemple après optimisation avec pagination** :
     ```java
     @GetMapping("/products")
     public Page<Product> getAllProducts(Pageable pageable) {
         return productService.getAllProducts(pageable);
     }
     ```
   - **Impact attendu** : Réduction de la taille des réponses HTTP, et meilleure gestion des grandes quantités de données.

3. **Traitement Asynchrone** : Utiliser des méthodes asynchrones pour les opérations longues ou qui nécessitent un temps de calcul important.
   - **Exemple avant optimisation** :
     ```java
     @GetMapping("/products/{id}")
     public Product getProduct(@PathVariable Long id) {
         return productService.getProductById(id);
     }
     ```
   - **Exemple après optimisation avec `@Async`** :
     ```java
     @Async
     @GetMapping("/products/{id}")
     public CompletableFuture<Product> getProduct(@PathVariable Long id) {
         return CompletableFuture.completedFuture(productService.getProductById(id));
     }
     ```
   - **Impact attendu** : Libération des threads HTTP pour traiter d'autres requêtes pendant le traitement des tâches lourdes.

#### **Bonnes Pratiques**

- **Utiliser le cache de manière sélective** : Ne cachez pas toutes les réponses, mais ciblez celles qui sont souvent demandées ou qui changent rarement.
- **Utiliser la pagination de manière efficace** : Ne surchargez pas le client avec des pages trop grandes.
- **Réduire le nombre de threads bloquants** : Utilisez des méthodes asynchrones pour les processus longs, sans surcharger le serveur.

---

### **Optimisation de la Couche Service**

#### **Introduction**

La couche **Service** est le cœur de la logique métier. Elle est responsable de la gestion des transactions, de la coordination des opérations entre la base de données et le contrôleur, et de l’implémentation des règles métiers. Pour garantir une performance optimale, il est essentiel de gérer efficacement les **transactions**, d’utiliser **l’exécution parallèle** pour les tâches indépendantes et d’adopter des stratégies de **traitement par lots** pour les opérations en masse.

#### **Enjeux et Défis**

Les défis de la couche Service sont nombreux. Il faut notamment garantir la **consistance transactionnelle**, gérer les **tâches parallèles** sans entraîner de conditions de course et optimiser les **traitements par lots** qui peuvent devenir coûteux en termes de ressources.

#### **Stratégies d’Optimisation**

1. **Gestion des Transactions avec `@Transactional`** : Cette annotation permet de gérer les transactions de manière déclarative.
   - **Exemple avant optimisation** :
     ```java
     public void updateProduct(Long productId, Product newProduct) {
         Product product = productRepository.findById(productId).get();
         product.setName(newProduct.getName());
         productRepository.save(product);
     }
     ```
   - **Exemple après optimisation avec `@Transactional`** :
     ```java
     @Transactional
     public void updateProduct(Long productId, Product newProduct) {
         Product product = productRepository.findById(productId).get();
         product.setName(newProduct.getName());
         productRepository.save(product);
     }
     ```
   - **Impact attendu** : Garantir l'intégrité des données en cas de défaillance.

2. **Exécution Parallèle avec `CompletableFuture`** : Permet d’exécuter des tâches indépendantes en parallèle.
   - **Exemple avant optimisation** :
     ```java
     public void processOrder(Order order) {
         processPayment(order.getPayment());
         processShipping(order.getShipping());
     }
     ```
   - **Exemple après optimisation avec `CompletableFuture`** :
     ```java
     public void processOrder(Order order) {
         CompletableFuture<Void> paymentFuture = CompletableFuture.runAsync(() -> processPayment(order.getPayment()));
         CompletableFuture<Void> shippingFuture = CompletableFuture.runAsync(() -> processShipping(order.getShipping()));
         CompletableFuture.allOf(paymentFuture, shippingFuture).join();
     }
     ```
   - **Impact attendu** : Réduction du temps de traitement global des commandes.

3. **Traitement par Lots** : Utiliser des mécanismes de traitement par lots pour optimiser les opérations qui nécessitent de traiter un grand nombre d’éléments.
   - **Exemple avant optimisation** :
     ```java
     public void updateProductPrices(List<Product> products) {
         for (Product product : products) {
             product.setPrice(product.getPrice() * 1.1);
             productRepository.save(product);
         }
     }
     ```
   - **Exemple après optimisation avec traitement par lots** :
     ```java
     @Transactional
     public void updateProductPrices(List<Product> products) {
         List<Product> updatedProducts = products.stream()
             .map(product -> {
                 product.setPrice(product.getPrice() * 1.1);
                 return product;
             })
             .collect(Collectors.toList());
         productRepository.saveAll(updatedProducts);
     }
     ```
   - **Impact attendu** : Diminution du nombre d'interactions avec la base de données, ce qui réduit les coûts et améliore les performances.

#### **Bonnes Pratiques**

- **Optimiser les transactions** : Limiter la durée des transactions et les opérations qu'elles couvrent.
- **Utiliser des traitements parallèles uniquement pour des tâches indépendantes**.
- **Gérer les erreurs dans les transactions** pour garantir la consistance des données.

---

### **Optimisation de la Couche Persistence**

#### **Introduction**

La couche **Persistence** est responsable de l’accès et de la gestion des données. L'optimisation de cette couche est essentielle pour éviter les **requêtes lentes**, le **problème N+1** et autres inefficacités liées à la gestion des entités. Il est également crucial d’activer des **mécanismes de mise en cache** pour réduire les accès répétitifs à la base de données et d’utiliser des **requêtes optimisées**.

#### **Enjeux et Défis**

Les défis principaux incluent l'optimisation des **requêtes complexes**, l'utilisation des **bons types de jointures** pour éviter le problème N+1, et la gestion efficace des **opérations de masse**.

#### **Stratégies d’Optimisation**

1. **Optimisation des Requêtes JPA pour éviter le Problème N+1** : Utiliser `JOIN FETCH` pour charger les entités associées en une seule requête.
   - **Exemple avant optimisation** :
     ```java
     @Query("SELECT p FROM Product p")
     List<Product> findAll();
     ```
   - **Exemple après optimisation avec `JOIN FETCH`** :
     ```java
     @Query("SELECT p FROM Product p JOIN FETCH p.category")
     List<Product> findAll();
     ```
   - **Impact attendu** : Réduction du nombre de requêtes envoyées à la base de données.

2. **Activation du Cache de Second Niveau avec Hibernate** : Cela permet de mettre en cache les entités fréquemment consultées.
   - **Exemple avant optimisation** :
     ```java
     @Entity
     public class Product

 {
         @Id
         private Long id;
         private String name;
     }
     ```
   - **Exemple après optimisation avec le cache de second niveau** :
     ```java
     @Entity
     @Cacheable
     public class Product {
         @Id
         private Long id;
         private String name;
     }
     ```
   - **Impact attendu** : Réduction de la charge sur la base de données pour les requêtes répétées.

3. **Optimisation des Requêtes avec des Index** : Utiliser des index sur les colonnes fréquemment utilisées dans les filtres de requêtes.
   - **Exemple avant optimisation** :
     ```java
     @Query("SELECT p FROM Product p WHERE p.name = ?1")
     List<Product> findByName(String name);
     ```
   - **Exemple après optimisation avec index** :
     ```sql
     CREATE INDEX idx_product_name ON product (name);
     ```
   - **Impact attendu** : Réduction des temps d’exécution des requêtes sur des grandes tables.

#### **Bonnes Pratiques**

- **Utiliser les jointures de manière optimale** pour éviter le problème N+1.
- **Activer le cache de second niveau** pour les entités fréquemment consultées.
- **Optimiser les index de base de données** en fonction des requêtes les plus fréquentes.

---

### **Conclusion**

Ces optimisations appliquées aux couches **Controller**, **Service** et **Persistence** permettent de renforcer les performances de votre application Spring Boot tout en garantissant la consistance des données et la réactivité du système. L'utilisation de JProfiler vous offre un excellent moyen d'analyser les performances en temps réel et d'identifier les goulots d'étranglement. En appliquant ces stratégies, vous pouvez non seulement améliorer l'expérience utilisateur, mais aussi réduire les coûts d’infrastructure et les risques de performance à grande échelle.



Voici un tableau détaillé des optimisations avant et après, avec les modifications dans les fichiers de configuration `.properties`, ainsi que les ajustements au niveau des classes Java.

| **Couche**        | **Stratégie d'Optimisation**              | **Exemple Avant Optimisation**                                                                 | **Exemple Après Optimisation**                                                                 | **Impact Attendu**                                                             | **Bonnes Pratiques**                                                                                         | **Modifications dans les fichiers `.properties`**  |
|-------------------|------------------------------------------|------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------|----------------------------------------------------|
| **Controller**     | **Mise en Cache**                        | `@GetMapping("/products/{id}") public Product getProduct(@PathVariable Long id) {...}`            | `@Cacheable(value = "productCache", key = "#id") @GetMapping("/products/{id}") {...}`              | Réduction de la charge sur la base de données, amélioration des temps de réponse | Cacher sélectivement les réponses les plus fréquemment demandées et celles qui changent rarement.           | `spring.cache.cache-names=productCache`  |
|                   | **Pagination**                           | `@GetMapping("/products") public List<Product> getAllProducts() {...}`                             | `@GetMapping("/products") public Page<Product> getAllProducts(Pageable pageable) {...}`             | Réduction de la taille des réponses HTTP, gestion plus efficace des grandes quantités de données               | Utiliser des tailles de pages raisonnables pour éviter de surcharger le client avec trop de données.        | `spring.data.web.pageable.default-page-size=20` |
|                   | **Traitement Asynchrone**                | `@GetMapping("/products/{id}") public Product getProduct(@PathVariable Long id) {...}`            | `@Async @GetMapping("/products/{id}") public CompletableFuture<Product> getProduct(...){...}`        | Libération des threads HTTP pour améliorer la réactivité en traitant les tâches longues de manière asynchrone | Utiliser l'asynchrone uniquement pour les tâches longues ou indépendantes afin de ne pas créer de goulots. | `spring.task.execution.pool.core-size=5`  |
| **Service**        | **Gestion des Transactions avec `@Transactional`** | `public void updateProduct(Long productId, Product newProduct) {...}`                             | `@Transactional public void updateProduct(Long productId, Product newProduct) {...}`                | Garantir l'intégrité des données en cas d'échec de transaction                       | Limiter la durée des transactions et les opérations qu'elles couvrent.                                       | `spring.jpa.properties.hibernate.flushMode=COMMIT` |
|                   | **Exécution Parallèle avec `CompletableFuture`** | `public void processOrder(Order order) {...}`                                                   | `public void processOrder(Order order) { CompletableFuture.runAsync(() -> processPayment(...)); ... }` | Réduction du temps de traitement des commandes en parallélisant les tâches indépendantes                    | Utiliser des tâches parallèles uniquement pour des processus indépendants pour éviter des conditions de course. | `spring.task.execution.pool.max-size=10`  |
|                   | **Traitement par Lots**                  | `public void updateProductPrices(List<Product> products) {...}`                                    | `@Transactional public void updateProductPrices(List<Product> products) { productRepository.saveAll(...)}` | Réduction du nombre d'interactions avec la base de données, amélioration des performances de traitement de masse | Regrouper les opérations pour réduire les accès multiples à la base de données.                            | `spring.batch.job.enabled=true`  |
| **Persistence**    | **Optimisation des Requêtes JPA (Problème N+1)**  | `@Query("SELECT p FROM Product p") List<Product> findAll();`                                       | `@Query("SELECT p FROM Product p JOIN FETCH p.category") List<Product> findAll();`                  | Réduction des requêtes envoyées à la base de données, évitement des problèmes de performances liés à N+1   | Utiliser des jointures `JOIN FETCH` pour charger les entités associées en une seule requête.               | `spring.jpa.properties.hibernate.default_batch_fetch_size=10` |
|                   | **Cache de Second Niveau avec Hibernate** | `@Entity public class Product {...}`                                                              | `@Entity @Cacheable public class Product {...}`                                                  | Réduction des accès à la base de données pour les données fréquemment consultées                                 | Activer le cache de second niveau pour les entités souvent lues, comme les produits populaires.             | `spring.jpa.properties.hibernate.cache.use_second_level_cache=true` |
|                   | **Optimisation des Requêtes avec des Index**  | `@Query("SELECT p FROM Product p WHERE p.name = ?1") List<Product> findByName(String name);`      | `CREATE INDEX idx_product_name ON product (name);`                                              | Réduction du temps d’exécution des requêtes pour les grandes tables et les colonnes fréquemment filtrées    | Ajouter des index sur les colonnes fréquemment filtrées pour accélérer les recherches.                      | `spring.datasource.hikari.connection-test-query=SELECT 1` |

---

### Explications détaillées des Modifications

#### 1. **Controller - Mise en Cache**
   - **Avant** : Les réponses pour chaque demande d'API, comme celle de récupérer un produit par son identifiant, font toujours une nouvelle requête à la base de données.
   - **Après** : On utilise l'annotation `@Cacheable` pour stocker le résultat de la requête dans un cache (ici nommé `productCache`). Cela permet de récupérer les produits rapidement sans interroger la base de données si la même demande a déjà été effectuée.
   - **Propriétés** : Ajout de la configuration `spring.cache.cache-names=productCache` dans le fichier `application.properties` pour définir un cache spécifique pour les produits.

#### 2. **Controller - Pagination**
   - **Avant** : L'API retourne une liste complète de tous les produits, ce qui peut être coûteux en termes de performance si la base de données contient un grand nombre de produits.
   - **Après** : L'API utilise la pagination via `Pageable`, ce qui permet de ne charger qu'une partie des produits à la fois, en fonction des paramètres de la page fournis.
   - **Propriétés** : Ajout de `spring.data.web.pageable.default-page-size=20` pour limiter la taille par défaut des pages à 20 éléments, réduisant ainsi la charge.

#### 3. **Controller - Traitement Asynchrone**
   - **Avant** : Les appels API bloquent le thread pendant le traitement des données, ce qui peut ralentir les performances, surtout pour des opérations longues comme la récupération de données complexes.
   - **Après** : L'utilisation de `@Async` permet de traiter ces opérations de manière asynchrone, libérant ainsi le thread principal et améliorant la réactivité de l'application.
   - **Propriétés** : Configuration de la taille du pool de tâches avec `spring.task.execution.pool.core-size=5`, ce qui permet de gérer l'exécution parallèle des tâches.

#### 4. **Service - Transactions avec `@Transactional`**
   - **Avant** : La gestion des transactions n'était pas explicite, et des erreurs pourraient survenir en cas de modifications multiples nécessitant une transaction atomique.
   - **Après** : L'annotation `@Transactional` garantit qu'une transaction est gérée de manière cohérente, ce qui permet de garantir l'intégrité des données.
   - **Propriétés** : `spring.jpa.properties.hibernate.flushMode=COMMIT` assure que les changements sont validés (flush) au moment où la transaction est explicitement validée.

#### 5. **Persistence - Optimisation des requêtes JPA**
   - **Avant** : Des requêtes JPA simples peuvent entraîner des problèmes de performance (N+1) lorsque des entités associées sont chargées par des requêtes supplémentaires.
   - **Après** : L'utilisation de `JOIN FETCH` permet de récupérer les entités associées dans la même requête, réduisant ainsi le nombre total de requêtes.
   - **Propriétés** : `spring.jpa.properties.hibernate.default_batch_fetch_size=10` optimise la récupération en lots, ce qui réduit les allers-retours à la base de données.

#### 6. **Persistence - Cache de Second Niveau avec Hibernate**
   - **Avant** : Les accès aux données fréquemment demandées sont toujours envoyés à la base de données, ce qui augmente la latence.
   - **Après** : En activant le cache de second niveau d'Hibernate, les données fréquemment demandées sont mises en cache, ce qui réduit la charge sur la base de données.
   - **Propriétés** : `spring.jpa.properties.hibernate.cache.use_second_level_cache=true` active ce cache dans Hibernate.

---

### Conclusion

Ces optimisations, tant au niveau de la couche de contrôleur que des services et de la persistance, permettent d'améliorer considérablement les performances de l'application. En utilisant des pratiques telles que la mise en cache, la gestion asynchrone, et la pagination, ainsi que des améliorations au niveau de la base de données (comme l'activation du cache de second niveau et la réduction du problème N+1), l'application devient plus rapide et plus évolutive. L'intégration des modifications dans le fichier `application.properties` permet une gestion facile des configurations liées aux caches, à la pagination et aux pools de tâches, assurant une meilleure optimisation globale de l'application.
