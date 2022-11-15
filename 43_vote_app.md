# Vote app demo

1. Commands that you used during the assignment
```sh
[root@ip-172-31-6-142 ~]# cd example-voting-app/
[root@ip-172-31-6-142 example-voting-app]# l
total 96
-rw-r--r-- 1 root root 54824 Nov 15 10:37 architecture.png
-rw-r--r-- 1 root root   609 Nov 15 10:37 dockercloud.yml
-rw-r--r-- 1 root root   808 Nov 15 10:37 docker-compose-javaworker.yml
-rw-r--r-- 1 root root   400 Nov 15 10:37 docker-compose-simple.yml
-rw-r--r-- 1 root root   808 Nov 15 10:37 docker-compose.yml
-rw-r--r-- 1 root root  1692 Nov 15 10:37 docker-stack.yml
drwxr-xr-x 2 root root   250 Nov 15 10:37 k8s-specifications
-rw-r--r-- 1 root root 10758 Nov 15 10:37 LICENSE
-rw-r--r-- 1 root root   185 Nov 15 10:37 MAINTAINERS
-rw-r--r-- 1 root root  2182 Nov 15 10:37 README.md
drwxr-xr-x 5 root root   133 Nov 15 10:37 result
drwxr-xr-x 4 root root    93 Nov 15 10:37 vote
drwxr-xr-x 3 root root    70 Nov 15 10:37 worker
[root@ip-172-31-6-142 example-voting-app]# kx apply -f k8s-specifications/
deployment.apps/db created
service/db created
deployment.apps/redis created
service/redis created
deployment.apps/result created
service/result created
deployment.apps/vote created
service/vote created
deployment.apps/worker created
[root@ip-172-31-6-142 example-voting-app]# kx get po -o wide
NAME                      READY   STATUS    RESTARTS   AGE   IP               NODE                                               NOMINATED NODE   READINESS GATES
db-b54cd94f4-vm5tf        1/1     Running   0          31s   192.168.80.150   ip-172-31-12-255.ap-southeast-1.compute.internal   <none>           <none>
redis-868d64d78-5swpf     1/1     Running   0          31s   192.168.80.151   ip-172-31-12-255.ap-southeast-1.compute.internal   <none>           <none>
result-5d57b59f4b-d2bzf   1/1     Running   0          31s   192.168.80.153   ip-172-31-12-255.ap-southeast-1.compute.internal   <none>           <none>
vote-94849dc97-h5m66      1/1     Running   0          31s   192.168.80.152   ip-172-31-12-255.ap-southeast-1.compute.internal   <none>           <none>
worker-dd46d7584-smc9m    1/1     Running   0          31s   192.168.80.154   ip-172-31-12-255.ap-southeast-1.compute.internal   <none>           <none>
[root@ip-172-31-6-142 example-voting-app]# alias kx
alias kx='kubectl --kubeconfig=/root/.kube/xconf'
[root@ip-172-31-6-142 example-voting-app]# cat ~/.kube/xconf | grep namespace
    namespace: xinfin
[root@ip-172-31-6-142 example-voting-app]#
```

2. snapshot of logs (wherever possible)
```db-log
[root@ip-172-31-6-142 example-voting-app]# kx logs db-b54cd94f4-vm5tf
The files belonging to this database system will be owned by user "postgres".
This user must also own the server process.

The database cluster will be initialized with locale "en_US.utf8".
The default database encoding has accordingly been set to "UTF8".
The default text search configuration will be set to "english".

Data page checksums are disabled.

fixing permissions on existing directory /var/lib/postgresql/data ... ok
creating subdirectories ... ok
selecting default max_connections ... 100
selecting default shared_buffers ... 128MB
creating configuration files ... ok
creating template1 database in /var/lib/postgresql/data/base/1 ... ok
initializing pg_authid ... ok
setting password ... ok
initializing dependencies ... ok
creating system views ... ok
loading system objects' descriptions ... ok
creating collations ... ok
creating conversions ... ok
creating dictionaries ... ok
setting privileges on built-in objects ... ok
creating information schema ... ok
loading PL/pgSQL server-side language ... ok
vacuuming database template1 ... ok
copying template1 to template0 ... ok
copying template1 to postgres ... ok
syncing data to disk ... ok

WARNING: enabling "trust" authentication for local connections
You can change this by editing pg_hba.conf or using the option -A, or
--auth-local and --auth-host, the next time you run initdb.

Success. You can now start the database server using:

    postgres -D /var/lib/postgresql/data
or
    pg_ctl -D /var/lib/postgresql/data -l logfile start

****************************************************
WARNING: No password has been set for the database.
         This will allow anyone with access to the
         Postgres port to access your database. In
         Docker's default configuration, this is
         effectively any other container on the same
         system.

         Use "-e POSTGRES_PASSWORD=password" to set
         it in "docker run".
****************************************************
waiting for server to start....LOG:  database system was shut down at 2022-11-15 10:48:42 UTC
LOG:  MultiXact member wraparound protections are now enabled
LOG:  database system is ready to accept connections
LOG:  autovacuum launcher started
 done
server started

/usr/local/bin/docker-entrypoint.sh: ignoring /docker-entrypoint-initdb.d/*

LOG:  received fast shutdown request
LOG:  aborting any active transactions
LOG:  autovacuum launcher shutting down
LOG:  shutting down
waiting for server to shut down....LOG:  database system is shut down
 done
server stopped

PostgreSQL init process complete; ready for start up.

LOG:  database system was shut down at 2022-11-15 10:48:43 UTC
LOG:  MultiXact member wraparound protections are now enabled
LOG:  database system is ready to accept connections
LOG:  autovacuum launcher started
ERROR:  relation "votes" does not exist at character 38
STATEMENT:  SELECT vote, COUNT(id) AS count FROM votes GROUP BY vote
```
```redis
[root@ip-172-31-6-142 example-voting-app]# kx logs redis-868d64d78-5swpf
1:C 15 Nov 2022 10:48:38.676 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
1:C 15 Nov 2022 10:48:38.676 # Redis version=7.0.5, bits=64, commit=00000000, modified=0, pid=1, just started
1:C 15 Nov 2022 10:48:38.676 # Warning: no config file specified, using the default config. In order to specify a config file use redis-server /path/to/redis.conf
1:M 15 Nov 2022 10:48:38.676 * monotonic clock: POSIX clock_gettime
1:M 15 Nov 2022 10:48:38.678 * Running mode=standalone, port=6379.
1:M 15 Nov 2022 10:48:38.685 # Server initialized
1:M 15 Nov 2022 10:48:38.685 # WARNING Your system is configured to use the 'xen' clocksource which might lead to degraded performance. Check the result of the [slow-clocksource] system check: run 'redis-server --check-system' to check if the system's clocksource isn't degrading performance.
1:M 15 Nov 2022 10:48:38.685 * Ready to accept connections
```
```result
[root@ip-172-31-6-142 example-voting-app]# kx logs result-5d57b59f4b-d2bzf
Tue, 15 Nov 2022 10:48:41 GMT body-parser deprecated bodyParser: use individual json/urlencoded middlewares at server.js:67:9
Tue, 15 Nov 2022 10:48:41 GMT body-parser deprecated undefined extended: provide extended option at ../node_modules/body-parser/index.js:105:29
App running on port 80
Waiting for db
Waiting for db
Waiting for db
Waiting for db
Connected to db
Error performing query: error: relation "votes" does not exist
```
```vote
[root@ip-172-31-6-142 example-voting-app]# kx logs vote-94849dc97-h5m66
[2022-11-15 10:48:40 +0000] [1] [INFO] Starting gunicorn 19.6.0
[2022-11-15 10:48:40 +0000] [1] [INFO] Listening at: http://0.0.0.0:80 (1)
[2022-11-15 10:48:40 +0000] [1] [INFO] Using worker: sync
[2022-11-15 10:48:40 +0000] [11] [INFO] Booting worker with pid: 11
[2022-11-15 10:48:40 +0000] [12] [INFO] Booting worker with pid: 12
[2022-11-15 10:48:40 +0000] [14] [INFO] Booting worker with pid: 14
[2022-11-15 10:48:40 +0000] [16] [INFO] Booting worker with pid: 16
```
```worker
[root@ip-172-31-6-142 example-voting-app]# kx logs worker-dd46d7584-smc9m 
Waiting for db
Waiting for db
Waiting for db
Connected to db
Found redis at 10.105.151.229
Connecting to redis
```

3. your comment on WHY result app STOPPED working after db pod stop

Result pod is recreated by the scheduler. Only data are lost from the database because the datastore of the db was stored locally in the pod which was destroyed, therefore the result app is defaulted to 50/50 - no votes.

4. your answer to HOW YOU MADE THE RESULT POD work
The result pod resumes working when db pod is recreated by the scheduler.

5. Some jargons (just names are enough- dont need sentences) that you learnt from the 40-hour Training session
Just some tags: #Image #Container #API #etcd #ConfigurationManagement #DataStore #KeyValueStore #Kubelet #Pod #Namespace #NodePort #clusterIP #ReplicaSet #DaemonSet #Taint #Toleration #SEP/ep #Scheduler #Node
