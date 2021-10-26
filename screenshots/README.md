# Screenshots
To help review your infrastructure, please include the following screenshots in this directory::

## Deployment Pipeline
* DockerHub showing containers that you have pushed
* GitHub repository’s settings showing your Travis webhook (can be found in Settings - Webhook)
* Travis CI showing a successful build and deploy job

## Kubernetes
* To verify Kubernetes pods are deployed properly
```bash
➜  udagram-microservices git:(master) ✗ kubectl get pods
NAME                            READY   STATUS    RESTARTS   AGE
backend-feed-5d48c47f49-bc8jk   1/1     Running   0          6m19s
backend-feed-5d48c47f49-m7xx4   1/1     Running   0          6m19s
backend-feed-5d48c47f49-tm2q5   1/1     Running   0          25m
backend-user-6dcd69d9d7-8j5tf   1/1     Running   0          6m18s
backend-user-6dcd69d9d7-dm954   1/1     Running   0          25m
frontend-f98c8979b-8n9tw        1/1     Running   0          6m17s
frontend-f98c8979b-dggcj        1/1     Running   0          6m15s
reverseproxy-5c6895fdcd-cj8hp   1/1     Running   0          6m16s
reverseproxy-5c6895fdcd-xp4vr   1/1     Running   0          25m
```
* To verify Kubernetes services are properly set up
```bash
➜  udagram-microservices git:(master) ✗ kubectl describe services
Name:              backend-feed
Namespace:         default
Labels:            service=backend-feed
Annotations:       kubectl.kubernetes.io/last-applied-configuration:
                     {"apiVersion":"v1","kind":"Service","metadata":{"annotations":{},"labels":{"service":"backend-feed"},"name":"backend-feed","namespace":"de...
Selector:          service=backend-feed
Type:              ClusterIP
IP:                10.100.142.42
Port:              8080  8080/TCP
TargetPort:        8080/TCP
Endpoints:         192.168.21.154:8080,192.168.75.25:8080,192.168.90.144:8080
Session Affinity:  None
Events:            <none>


Name:              backend-user
Namespace:         default
Labels:            service=backend-user
Annotations:       kubectl.kubernetes.io/last-applied-configuration:
                     {"apiVersion":"v1","kind":"Service","metadata":{"annotations":{},"labels":{"service":"backend-user"},"name":"backend-user","namespace":"de...
Selector:          service=backend-user
Type:              ClusterIP
IP:                10.100.20.141
Port:              8080  8080/TCP
TargetPort:        8080/TCP
Endpoints:         192.168.8.153:8080,192.168.86.111:8080
Session Affinity:  None
Events:            <none>


Name:              frontend
Namespace:         default
Labels:            service=frontend
Annotations:       kubectl.kubernetes.io/last-applied-configuration:
                     {"apiVersion":"v1","kind":"Service","metadata":{"annotations":{},"labels":{"service":"frontend"},"name":"frontend","namespace":"default"},...
Selector:          service=frontend
Type:              ClusterIP
IP:                10.100.223.205
Port:              8100  8100/TCP
TargetPort:        80/TCP
Endpoints:         192.168.30.210:80,192.168.94.141:80
Session Affinity:  None
Events:            <none>


Name:              kubernetes
Namespace:         default
Labels:            component=apiserver
                   provider=kubernetes
Annotations:       <none>
Selector:          <none>
Type:              ClusterIP
IP:                10.100.0.1
Port:              https  443/TCP
TargetPort:        443/TCP
Endpoints:         192.168.127.135:443,192.168.134.183:443
Session Affinity:  None
Events:            <none>


Name:                     publicfrontend
Namespace:                default
Labels:                   service=frontend
Annotations:              <none>
Selector:                 service=frontend
Type:                     LoadBalancer
IP:                       10.100.56.57
LoadBalancer Ingress:     a820d5c89100e468ca6f914bc3adb4f2-1783643409.us-east-2.elb.amazonaws.com
Port:                     <unset>  80/TCP
TargetPort:               80/TCP
NodePort:                 <unset>  31378/TCP
Endpoints:                192.168.30.210:80,192.168.94.141:80
Session Affinity:         None
External Traffic Policy:  Cluster
Events:
  Type    Reason                Age   From                Message
  ----    ------                ----  ----                -------
  Normal  EnsuringLoadBalancer  19m   service-controller  Ensuring load balancer
  Normal  EnsuredLoadBalancer   19m   service-controller  Ensured load balancer


Name:                     publicreverseproxy
Namespace:                default
Labels:                   service=reverseproxy
Annotations:              <none>
Selector:                 service=reverseproxy
Type:                     LoadBalancer
IP:                       10.100.224.234
LoadBalancer Ingress:     a673ba17db808413da74eb11f1f3cf19-1118841903.us-east-2.elb.amazonaws.com
Port:                     <unset>  8080/TCP
TargetPort:               8080/TCP
NodePort:                 <unset>  30638/TCP
Endpoints:                192.168.0.113:8080,192.168.84.48:8080
Session Affinity:         None
External Traffic Policy:  Cluster
Events:
  Type    Reason                Age   From                Message
  ----    ------                ----  ----                -------
  Normal  EnsuringLoadBalancer  19m   service-controller  Ensuring load balancer
  Normal  EnsuredLoadBalancer   19m   service-controller  Ensured load balancer


Name:                     reverseproxy
Namespace:                default
Labels:                   service=reverseproxy
Annotations:              kubectl.kubernetes.io/last-applied-configuration:
                            {"apiVersion":"v1","kind":"Service","metadata":{"annotations":{},"labels":{"service":"reverseproxy"},"name":"reverseproxy","namespace":"de...
Selector:                 service=reverseproxy
Type:                     LoadBalancer
IP:                       10.100.44.76
LoadBalancer Ingress:     ae287ec932a794207b22065a434b742d-1644228110.us-east-2.elb.amazonaws.com
Port:                     8080  8080/TCP
TargetPort:               8080/TCP
NodePort:                 8080  31337/TCP
Endpoints:                192.168.0.113:8080,192.168.84.48:8080
Session Affinity:         None
External Traffic Policy:  Cluster
Events:
  Type    Reason                Age   From                Message
  ----    ------                ----  ----                -------
  Normal  EnsuringLoadBalancer  26m   service-controller  Ensuring load balancer
  Normal  EnsuredLoadBalancer   26m   service-controller  Ensured load balancer
```
* To verify that you have horizontal scaling set against CPU usage
```bash
➜  udagram-microservices git:(master) ✗ kubectl describe hpa
Name:                                                  backend-feed
Namespace:                                             default
Labels:                                                <none>
Annotations:                                           <none>
CreationTimestamp:                                     Tue, 26 Oct 2021 22:47:34 +0100
Reference:                                             Deployment/backend-feed
Metrics:                                               ( current / target )
  resource cpu on pods  (as a percentage of request):  <unknown> / 70%
Min replicas:                                          4
Max replicas:                                          6
Deployment pods:                                       3 current / 4 desired
Conditions:
  Type         Status  Reason            Message
  ----         ------  ------            -------
  AbleToScale  True    SucceededRescale  the HPA controller was able to update the target scale to 4
Events:
  Type    Reason             Age   From                       Message
  ----    ------             ----  ----                       -------
  Normal  SuccessfulRescale  3s    horizontal-pod-autoscaler  New size: 4; reason: Current number of replicas below Spec.MinReplicas
```
* To verify that you have set up logging with a backend application
```bash
➜  udagram-microservices git:(master) ✗ kubectl logs backend-feed-5d48c47f49-tm2q5

> udagram-api@2.0.0 prod /usr/src/app
> tsc && node ./www/server.js

Initialize database connection...
Executing (default): CREATE TABLE IF NOT EXISTS "FeedItem" ("id"   SERIAL , "caption" VARCHAR(255), "url" VARCHAR(255), "createdAt" TIMESTAMP WITH TIME ZONE, "updatedAt" TIMESTAMP WITH TIME ZONE, PRIMARY KEY ("id"));
Executing (default): SELECT i.relname AS name, ix.indisprimary AS primary, ix.indisunique AS unique, ix.indkey AS indkey, array_agg(a.attnum) as column_indexes, array_agg(a.attname) AS column_names, pg_get_indexdef(ix.indexrelid) AS definition FROM pg_class t, pg_class i, pg_index ix, pg_attribute a WHERE t.oid = ix.indrelid AND i.oid = ix.indexrelid AND a.attrelid = t.oid AND t.relkind = 'r' and t.relname = 'FeedItem' GROUP BY i.relname, ix.indexrelid, ix.indisprimary, ix.indisunique, ix.indkey ORDER BY i.relname;
server running http://localhost:8100
press CTRL+C to stop server
Executing (default): SELECT count(*) AS "count" FROM "FeedItem" AS "FeedItem";
Executing (default): SELECT "id", "caption", "url", "createdAt", "updatedAt" FROM "FeedItem" AS "FeedItem" ORDER BY "FeedItem"."id" DESC;
Executing (default): INSERT INTO "FeedItem" ("id","caption","url","createdAt","updatedAt") VALUES (DEFAULT,$1,$2,$3,$4) RETURNING *;
```
