# Formation: Optimisation et Profiling des Applications Java avec NetBeans Profiler et JProfiler
Formation Optimisation et Profiling des applications JAVA avec JProfiler ou NetBean Profiler

#### Objectifs
**Objectifs Opérationnels:**
- Savoir monitorer et profiler les applications Java.
- Expliciter les techniques permettant d'optimiser les applications.

**Objectifs Pédagogiques:**
À l'issue de cette formation, les participants auront acquis les compétences nécessaires pour:
- Maîtriser les concepts fondamentaux du langage Java (gestion de la mémoire, mécanisme d'exécution, chargement de classes, etc.).
- Connaître les impacts de l'algorithmie sur les performances.
- Maîtriser les techniques classiques d'optimisation.
- Connaître les particularités des services back-end écrits en Java.

#### Public Cible
- Développeurs, chefs de projet ou architectes impliqués dans la réalisation d'applications Java, particulièrement J2EE.

#### Prérequis
- Connaissance de Java.

#### Contenu du Cours

**Jour 1: Introduction et Fondamentaux**
1. **Introduction**
   - Optimisation de performance: méthodologie, recommandations, benchmarking, métriques observées.

2. **Concepts Fondamentaux de Java**
   - JVM, zones mémoires et GC, ClassLoader, multi-threading, JIT, JVMTI, outils de base.
   - Causes de mauvaises performances: CPU, mémoire, IO.
   - Le ramasse-miettes: pools de la HotSpot, différents algorithmes, tuning du garbage collector.

3. **Atelier Pratique**
   - Observation des collectes avec JProfiler, tuning des zones mémoire et de l’algorithme.

**Jour 2: Outils et Techniques d'Optimisation**
1. **Boîte à Outils**
   - Outils systèmes: JMX, Mxbeans de la JVM, agents JVMTI.
   - Outils fournis par la distribution: commandes en ligne Java, jVisualVM, jmc.
   - JProfiler: distributions, fonctionnalités (enregistrements des appels, CPU profiling, mémoire, multi-threading).

2. **Atelier Pratique**
   - Monitoring via des consoles JMX, exemple d’un agent JVMTI.
   - Ateliers avec les commandes en ligne.
   - Utilisation de JProfiler pour différents cas de détection de mauvais fonctionnement.

3. **Optimisation de Code Java**
   - Limitation d'instances temporaires, boucles et récursivité, streams.
   - Utilisation des chaînes de caractères, switch, exceptions et stacktrace.
   - Gestion des I/O, bufferisation, package java.nio, gestion des traces.

4. **Collections et Tableaux**
   - Choisir les bonnes implémentations en fonction de l’algorithme.
   - Collections synchronisées ou non.
   - Coût des allocations/désallocations: réutilisation d'instance, pattern pool, singleton, ThreadLocal, weak references.

5. **Applications Multithreadées**
   - Cas d'usage des threads, problèmes de synchronisation, mécanismes de base.
   - Package java.util.concurrent, utilisation de pool de threads, Java 8 et l’asynchronisme, reactive programming.

6. **Atelier Pratique**
   - Optimisation d’application en utilisant les techniques présentées.

**Jour 3: Services Back-End et Travaux Pratiques**
1. **Particularités des Services Back-End JavaEE**
   - Différents pools de l’architecture, caches, scalabilité.
   - Outils de simulation de charge, intégration des serveurs JProfiler.

2. **Persistance et JPA**
   - Pools de connexions, optimisation du schéma, caches.

3. **Métier**
   - Modèle stateless/stateful, transactions.

4. **Http et REST**
   - Sérialisation/désérialisation, optimisation des transferts.

5. **Ateliers Pratiques**
   - Diagnostic de problèmes sur une application web complète, sur une API Rest.
   - Utilisation de NetBeans, NetBeans Profiler ou JProfiler.

Cette formation vous permettra de devenir compétent dans le profilage et l’optimisation des applications Java, en utilisant des outils avancés comme JProfiler et NetBeans Profiler.
