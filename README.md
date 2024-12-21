# postgresql-monitoring
Dans cet article, nous allons explorer le monitoring de PostgreSQL en abordant les points suivants :
## 1. logging_collector
## 2. PgBadger
## 3. PG_STAT_STATEMENTS
----------------------------------------------------------------------
#  1- logging_collector
Le paramètre **`logging_collector`** dans PostgreSQL est utilisé pour activer la collecte et l'enregistrement des messages de journal dans des fichiers spécifiques plutôt que d'être envoyés simplement à la sortie standard (`stderr`). Ce mécanisme de collecte est un processus en arrière-plan qui redirige les messages de journal vers des fichiers, ce qui facilite la gestion et l'analyse des journaux de PostgreSQL.

### Activation de `logging_collector`
Pour activer la collecte des journaux, vous devez définir **`logging_collector`** à **`on`** dans le fichier de configuration **`postgresql.conf`**. Voici comment procéder :

```bash
logging_collector = on
```

### Paramètres associés à `logging_collector`

1. **`log_directory`**  
   Définit le répertoire dans lequel les fichiers journaux seront créés. Vous pouvez spécifier un chemin absolu ou relatif par rapport au répertoire de données de PostgreSQL.
   
   Exemple :
   ```bash
   log_directory = '/var/log/postgresql'
   ```
   
2. **`log_filename`**  
   Détermine le nom des fichiers journaux créés. Vous pouvez utiliser des variables pour générer des noms de fichiers dynamiques, par exemple :  
   - **`%H`** : Heure de la journée
   - **`%a`** : Jour de la semaine
   - **`%d`** : Jour du mois

   Exemple de configuration de nom de fichier :
   ```bash
   log_filename = 'postgresql-%H.log'
   ```

3. **`log_rotation_age`**  
   Définit la fréquence à laquelle les fichiers journaux doivent être archivés, en minutes. Après ce délai, un nouveau fichier journal sera créé.

   Exemple :
   ```bash
   log_rotation_age = 1440  # 1 jour
   ```

4. **`log_rotation_size`**  
   Cette option permet de limiter la taille d'un fichier journal avant qu'un nouveau fichier ne soit créé. Par exemple, si vous voulez créer un nouveau fichier journal chaque fois que celui-ci atteint 100 Mo :

   ```bash
   log_rotation_size = 100MB
   ```

### Pourquoi activer `logging_collector` ?

- **Centralisation des logs** : Permet de collecter les journaux dans un répertoire centralisé pour une gestion plus facile.
- **Analyse des performances** : Avec des journaux collectés, il est possible d'analyser les requêtes lentes, les erreurs, les checkpoints, etc.
- **Sécurité et audit** : Enregistrer les connexions et déconnexions permet un meilleur suivi des accès à la base de données.
- **Dépannage** : Les journaux peuvent aider à diagnostiquer des problèmes de performance ou des erreurs de requêtes. 

Il est important de configurer correctement les répertoires et noms de fichiers de manière à éviter d'éventuels problèmes de stockage et à faciliter l'archivage et la récupération des journaux.
![image](https://github.com/user-attachments/assets/9a87c461-698a-4942-bc9f-7004853d3cb1)

# 2- PgBadger
**PgBadger** est un outil open-source de génération de rapports d'analyse des logs PostgreSQL. Il est utilisé pour analyser les fichiers journaux de PostgreSQL et produire des rapports détaillés sur les performances et les requêtes, ce qui permet aux administrateurs de bases de données d'identifier facilement les problèmes de performance, d'optimiser les requêtes lentes et d'améliorer la gestion de la base de données.

### Fonctionnalités principales de PgBadger :

1. **Analyse des logs PostgreSQL** :
   PgBadger prend en charge l'analyse des fichiers journaux générés par PostgreSQL lorsqu'il est configuré pour collecter les logs. Ces fichiers peuvent être au format texte brut ou au format CSV.

2. **Rapports de performance** :
   Il génère des rapports sur les performances des requêtes en fonction de leur durée d'exécution. Cela permet de repérer les requêtes lentes, les goulets d'étranglement et les zones à optimiser.

3. **Vue d'ensemble des requêtes** :
   PgBadger fournit des informations sur les requêtes les plus fréquentes, les plus lentes et celles qui consomment le plus de ressources, telles que le CPU et la mémoire.

4. **Visualisation des statistiques de base de données** :
   Les rapports incluent des graphiques interactifs et des statistiques détaillées sur les connexions, les erreurs, les écritures et les lectures de la base de données, les checkpoints, etc.

5. **Détection des erreurs et des avertissements** :
   PgBadger détecte les erreurs courantes dans les logs de PostgreSQL (par exemple, les erreurs de requête, les problèmes de verrouillage, etc.) et les met en évidence dans le rapport.

6. **Support des logs au format texte et CSV** :
   PgBadger peut traiter des logs PostgreSQL au format texte standard ou au format CSV, ce qui le rend flexible pour différentes configurations de logs.

7. **Rapports HTML et PDF** :
   Les résultats sont présentés dans un rapport HTML interactif, mais PgBadger permet également d'exporter ces rapports sous forme de PDF pour une présentation ou une analyse hors ligne.

### Avantages de PgBadger :
- **Optimisation des performances** : En analysant les requêtes lentes, PgBadger permet d'identifier des pistes pour optimiser les performances de la base de données.
- **Facilité d'utilisation** : C'est un outil simple à configurer et à utiliser, avec une interface de rapport claire et détaillée.
- **Accélération de la résolution des problèmes** : Il aide les administrateurs à détecter rapidement les problèmes dans les logs, ce qui accélère la résolution des problèmes de performance ou d'erreur.
- **Analyse complète** : PgBadger offre une vue d'ensemble détaillée de l'activité de la base de données, y compris les requêtes, les utilisateurs, les erreurs, etc.
### Installation et génération de rapports étape par étape :

**Télécharger PgBadger** :  
[https://github.com/darold/pgbadger/releases](https://github.com/darold/pgbadger/releases)

**Prérequis** :  
```bash
yum install perl-devel
```
Et sur un système RPM-like, utilisez :
```bash
sudo yum install perl-JSON-XS - sudo yum install perl-text-csv_xs
```

#### Étapes d'installation :

1. Décompressez l'archive :
```bash
tar -xzf pgbadger-12.4.tar.gz
```
![image](https://github.com/user-attachments/assets/5ff3f56d-e37f-4353-8c3c-19b94fdcc654)

2. Accédez au répertoire PgBadger :
```bash
cd pgbadger-12.4
```

3. Exécutez la commande suivante pour préparer l'installation :
```bash
perl Makefile.PL
![image](https://github.com/user-attachments/assets/8a7224f4-9918-400a-a262-0248a0c47f76)


```
Sortie d'exemple :
```bash
[postgres@standby pgbadger-12.4]$ perl Makefile.PL
Checking if your kit is complete...
Looks good
Generating a Unix-style Makefile
Writing Makefile for pgBadger
Writing MYMETA.yml and MYMETA.json
```

4. Compilez et installez :
```bash
make && sudo make install
```
![image](https://github.com/user-attachments/assets/8ad5d57c-2805-43c5-8ab7-6f043e0b0ab4)

5. **Configurer les variables d'environnement** :
```bash
LD_LIBRARY_PATH=/usr/pgsql-16/lib
export LD_LIBRARY_PATH

PATH=/usr/pgsql-16/bin:$PATH
export PATH

export PGDATA='/var/lib/pgsql/16/data'
export PGLOG='/var/lib/pgsql/16/data/log'
export PGBADGER='/var/lib/pgsql/16/pgbadger/pgbadger-12.4'
export PGREPORTS='/var/lib/pgsql/16/reports'

./pgbadger --help
```
![image](https://github.com/user-attachments/assets/b3204901-d483-406a-a38e-0b60cd3a89c4)

6. **Paramétrer PostgreSQL** :  
Connectez-vous à `psql` et configurez les paramètres de journalisation :

```sql
alter system set logging_collector = 'on';
alter system set log_truncate_on_rotation = 'on';
alter system set log_rotation_age = 1440;
alter system set log_filename = 'postgresql-%a.log';
```

**Que journaliser :**
```sql
alter system set log_line_prefix = '%t [%p]: user=%u,db=%d,app=%a,client=%h';
alter system set log_checkpoints ='on';
alter system set log_connections ='on';
alter system set log_disconnections ='on';
alter system set log_lock_waits ='on';
alter system set log_temp_files = 0;
alter system set log_autovacuum_min_duration = 0;
alter system set log_min_duration_statement='50ms';
alter system set deadlock_timeout = '1s';
alter system set log_error_verbosity = 'terse';
SELECT pg_reload_conf();
```

7. **Générer une charge de travail (facultatif)** :
```bash
pgbench -c 10 -j 2 -t 1000 postgres
```
![image](https://github.com/user-attachments/assets/23c7e5b7-7f0e-4d7d-8f0a-0ae0a000f47a)

![image](https://github.com/user-attachments/assets/8fed989f-ad4f-48c8-8edb-000944d35dc5)


8. **Générer des rapports** :
```bash
./pgbadger /var/lib/pgsql/16/data/log/postgres* -O /var/lib/pgsql/16/reports
```
![image](https://github.com/user-attachments/assets/20070a89-f39a-46e1-890f-a4598998e0b8)


Avec un fichier de sortie spécifique :
```bash
./pgbadger /var/lib/pgsql/16/data/log/postgres* -o /var/lib/pgsql/16/reports/today_report.html
```
![image](https://github.com/user-attachments/assets/8b12c323-3d7d-4967-a8b6-771d19598736)


**Rapport d'erreurs** :
```bash
./pgbadger -q -w /var/lib/pgsql/16/data/log/postgres* -o /var/lib/pgsql/16/reports/today_report.html
```
![image](https://github.com/user-attachments/assets/0d3a7c7b-35dc-4300-9b5f-d4de8d4594ff)

Pour analyser les logs de manière incrémentielle et ajouter les résultats à un rapport unique, utilisez les options `--last-parsed` et `--outfile` :
```bash
./pgbadger /var/lib/pgsql/16/data/log/postgresql*.log --last-parsed /var/lib/pgsql/16/reports/pgbadger_last_state_file --outfile /var/lib/pgsql/16/reports/report.html
```
![image](https://github.com/user-attachments/assets/52bb63f6-7d1d-438c-8b08-133ba3983dac)
![image](https://github.com/user-attachments/assets/78c863db-6e19-4181-bd56-7dd3e6680268)

![image](https://github.com/user-attachments/assets/e3720601-ce9e-4272-ae85-e94ff48a2629)
![image](https://github.com/user-attachments/assets/e4546d59-3f22-4476-99f4-32abe5e8466a)
![image](https://github.com/user-attachments/assets/a3de3d96-b1cf-468e-bef2-6854fdd8c59d)
![image](https://github.com/user-attachments/assets/b601f7b4-10de-422a-abc8-e6a8bf1dd062)
![image](https://github.com/user-attachments/assets/3dceb5e2-1241-4038-ba81-af7f52c152fa)
![image](https://github.com/user-attachments/assets/67e8eb34-44cf-487d-8673-df057eb2107a)
![image](https://github.com/user-attachments/assets/4a3cb96c-0239-413b-aa2e-da67a409f035)


**Planification automatique des rapports incrémentiels avec cron (hebdomadaire)** :
```bash
0 4 * * * /var/lib/pgsql/16/pgbadger/pgbadger-12.4/pgbadger -I -q /var/lib/pgsql/16/data/log/postgresql*.log -O /var/lib/pgsql/16/reports/
```

Par défaut, PgBadger génère un rapport HTML. Toutefois, vous pouvez choisir d'autres formats de sortie (CSV ou JSON) en utilisant l'option `--format` :
```bash
pgbadger /path/to/postgresql.log -o report.csv --format csv
```
**Analyse**
![image](https://github.com/user-attachments/assets/9c15cc9e-3e45-43b9-9d35-da2914145c3d)
![image](https://github.com/user-attachments/assets/c9b8ccbe-0669-4f23-8093-d353ca7c46bc)
![image](https://github.com/user-attachments/assets/a0f6fc92-7410-44fc-8c87-b55780da2b22)

### 3- **pg_stat_statements**

Le module `pg_stat_statements` fournit un moyen de suivre les statistiques de planification et d'exécution de toutes les requêtes SQL. Cette extension n'est pas activée par défaut globalement, mais peut être activée pour une base de données spécifique en utilisant la commande `CREATE EXTENSION pg_stat_statements`.

#### **Configuration :**

- `shared_preload_libraries = 'pg_stat_statements'`  
  Cette directive charge le module `pg_stat_statements` lors du démarrage de PostgreSQL.
  
- `pg_stat_statements.max = 10000`  
  Définit le nombre maximum de requêtes que le module peut suivre.

- `pg_stat_statements.track = all`  
  Contrôle le niveau de suivi des requêtes. Les options possibles sont :  
  - `top` : suit les requêtes émises directement par les clients.  
  - `all` : suit les requêtes de niveau supérieur et les requêtes imbriquées.  
  - `none` : désactive la collecte des statistiques des requêtes.

- `track_activity_query_size = 2048`  
  Définit le nombre maximal de caractères à afficher lors de la génération des rapports d'une requête SQL. La valeur par défaut est de 1024 octets.

- `track_io_timing = on`  
  Active le suivi des appels I/O de la base de données. Ce paramètre est désactivé par défaut car il peut entraîner une surcharge importante sur certaines plateformes en raison des requêtes fréquentes vers le système d'exploitation pour obtenir l'heure actuelle.

#### **Fonctions associées :**

- **PG_STAT_STATEMENTS.MAX :**  
  Nombre maximal de requêtes SQL suivies par le module.

- **PG_STAT_STATEMENTS.TRACK :**  
  Contrôle quelles requêtes sont suivies. Les options disponibles incluent `top`, `all`, et `none`.

- **Track_IO_Timing :**  
  Permet le suivi des temps des appels I/O dans la base de données. Ce paramètre est désactivé par défaut, car il peut entraîner une surcharge significative en fonction de la plateforme.

- **Track_Activity_Query_Size :**  
  Définit le nombre de caractères à afficher lors de l'affichage d'une requête SQL. Par défaut, ce nombre est de 1024 octets.

#### **PG_STAT_STATEMENT_RESET :**

Cette fonction permet de réinitialiser toutes les statistiques recueillies jusqu'à présent par `pg_stat_statements`. Par défaut, cette fonction ne peut être exécutée que par des superutilisateurs.



### **1. Trouver les tables avec trop de tuples morts (bloated tables)**

```sql
select 
  schemaname, 
  relname, 
  n_tup_ins, 
  n_tup_upd, 
  n_tup_del, 
  n_live_tup, 
  n_dead_tup, 
  DATE_TRUNC('minute', last_vacuum) last_vacuum, 
  DATE_TRUNC('minute', last_autovacuum) last_autovacuum
from 
  pg_stat_all_tables 
where 
  schemaname = 'public'
order by 
  n_dead_tup desc;
```
Cette requête affiche des informations sur les tables du schéma public, y compris le nombre de tuples vivants et morts. Elle aide à identifier les tables nécessitant une opération de nettoyage (`VACUUM`).

### **2. Obtenir les index des tables**

```sql
select
    t.relname as table_name,
    i.relname as index_name,
    string_agg(a.attname, ',') as column_name
from
    pg_class t,
    pg_class i,
    pg_index ix,
    pg_attribute a
where
    t.oid = ix.indrelid
    and i.oid = ix.indexrelid
    and a.attrelid = t.oid
    and a.attnum = ANY(ix.indkey)
    and t.relkind = 'r'
    and t.relname not like 'pg_%'
group by  
    t.relname,
    i.relname
order by
    t.relname,
    i.relname;
```
Cette requête liste les tables et leurs index associés, ainsi que les colonnes utilisées par chaque index.

### **3. Requêtes en cours d'exécution**

- **Requêtes en cours :**

```sql
SELECT pid, age(query_start, clock_timestamp()), usename, query  
FROM pg_stat_activity 
WHERE query != '<IDLE>' AND query NOT ILIKE '%pg_stat_activity%' 
ORDER BY query_start desc;
```

- **Requêtes qui s'exécutent depuis plus de 2 minutes :**

```sql
SELECT now() - query_start as "runtime", usename, datname, state, query 
FROM pg_stat_activity 
WHERE now() - query_start > '2 minutes'::interval 
ORDER BY runtime DESC;
```

- **Requêtes qui s'exécutent depuis plus de 9 secondes :**

```sql
SELECT now() - query_start as "runtime", usename, datname, state, query 
FROM pg_stat_activity 
WHERE now() - query_start > '9 seconds'::interval 
ORDER BY runtime DESC;
```

- **Annuler une requête en cours :**

```sql
SELECT pg_cancel_backend(procpid);
```

- **Annuler une requête en veille :**

```sql
SELECT pg_terminate_backend(procpid);
```

- **Exécuter une commande `VACUUM` :**

```sql
VACUUM (VERBOSE, ANALYZE);
```

### **4. Intégrité des données**

- **Taux de cache :**

```sql
select sum(blks_hit)*100/sum(blks_hit+blks_read) as hit_ratio from pg_stat_database;
```

- **Taille des tables :**

```sql
select relname, pg_size_pretty(pg_total_relation_size(relname::regclass)) as full_size, 
       pg_size_pretty(pg_relation_size(relname::regclass)) as table_size, 
       pg_size_pretty(pg_total_relation_size(relname::regclass) - pg_relation_size(relname::regclass)) as index_size 
from pg_stat_user_tables 
order by pg_total_relation_size(relname::regclass) desc limit 10;
```

- **Taille des bases de données :**

```sql
select datname, pg_size_pretty(pg_database_size(datname)) 
from pg_database 
order by pg_database_size(datname);
```

- **Index inutilisés :**

```sql
select * from pg_stat_all_indexes where idx_scan = 0;
```

- **Activité d'écriture (utilisation des index) :**

```sql
select s.relname, pg_size_pretty(pg_relation_size(relid)), 
       coalesce(n_tup_ins,0) + 2 * coalesce(n_tup_upd,0) - coalesce(n_tup_hot_upd,0) + coalesce(n_tup_del,0) AS total_writes, 
       (coalesce(n_tup_hot_upd,0)::float * 100 / (case when n_tup_upd > 0 then n_tup_upd else 1 end)::float)::numeric(10,2) AS hot_rate, 
       (select v[1] FROM regexp_matches(reloptions::text,E'fillfactor=(d+)') as r(v) limit 1) AS fillfactor 
from pg_stat_all_tables s 
join pg_class c ON c.oid=relid 
order by total_writes desc limit 50;
```

- **Les tables nécessitant un index :**

```sql
SELECT relname, seq_scan-idx_scan AS too_much_seq, 
       CASE WHEN seq_scan-idx_scan>0 THEN 'Missing Index?' ELSE 'OK' END, 
       pg_relation_size(relname::regclass) AS rel_size, seq_scan, idx_scan 
FROM pg_stat_all_tables 
WHERE schemaname='public' AND pg_relation_size(relname::regclass)>80000 
ORDER BY too_much_seq DESC;
```

### **5. Activité de la base**

- **Connexions d'utilisateurs :**

```sql
select count(*)*100/(select current_setting('max_connections')::int) 
from pg_stat_activity;
```

- **Temps moyen d'exécution des requêtes :**

```sql
select (sum(total_time) / sum(calls))::numeric(6,3) 
from pg_stat_statements;
```

- **Les requêtes qui écrivent le plus dans les buffers partagés :**

```sql
select query, shared_blks_dirtied 
from pg_stat_statements 
where shared_blks_dirtied > 0 
order by 2 desc;
```

- **Temps de lecture des blocs (Block Read Time) :**

```sql
select * from pg_stat_statements where blk_read_time <> 0 order by blk_read_time desc;
```

### **6. Vacuuming**

- **Dernière opération de `VACUUM` et `ANALYZE` :**

```sql
select relname,last_vacuum, last_autovacuum, last_analyze, last_autoanalyze from pg_stat_user_tables;
```

- **Nombre total de tuples morts devant être nettoyés par table :**

```sql
select n_dead_tup, schemaname, relname from pg_stat_all_tables;
```

- **Nombre total de tuples morts dans la base de données devant être nettoyés :**

```sql
select sum(n_dead_tup) from pg_stat_all_tables;
``` 

Ces requêtes permettent d'optimiser et de suivre l'état de la base de données, de surveiller les requêtes en cours, et de prendre des mesures appropriées pour maintenir la performance et la santé du système PostgreSQL.


