# Day 59 – Helm — Kubernetes Package Manager

## Task 1: Install Helm

### Install Helm using brew, the curl script, or Chocolatey depending on your OS, then verify the installation with helm version and helm env

```bash

ubuntu@ip-172-31-6-80:~$ helm version
version.BuildInfo{Version:"v4.3.0", GitCommit:"bec5b06ed841fe5269972d864d5177944fd5970f", GitTreeState:"clean", GoVersion:"go1.27.1", KubeClientVersion:"v1.37"}
ubuntu@ip-172-31-6-80:~$ which helm
/usr/local/bin/helm
ubuntu@ip-172-31-6-80:~$

```

### Verify: What version of Helm is installed?

Helm version: v4.3.0
Installation path: /usr/local/bin/helm


## Task 2: Add a Repository and Search

### Add the Bitnami repository using helm repo add bitnami https://charts.bitnami.com/bitnami, update it with helm repo update, and search for charts using helm search repo nginx and helm search repo bitnami.

```bash

ubuntu@ip-172-31-6-80:~$ helm repo add bitnami https://charts.bitnami.com/bitnami

"bitnami" has been added to your repositories
ubuntu@ip-172-31-6-80:~$
ubuntu@ip-172-31-6-80:~$ helm repo update
Hang tight while we grab the latest from your chart repositories...
...Successfully got an update from the "bitnami" chart repository
Update Complete. ⎈Happy Helming!⎈
ubuntu@ip-172-31-6-80:~$ helm search repo nginx
NAME                                    CHART VERSION   APP VERSION     DESCRIPTION
bitnami/nginx                           25.1.14         1.31.6          NGINX Open Source is a web server that can be a...
bitnami/nginx-ingress-controller        12.0.7          1.13.1          NGINX Ingress Controller is an Ingress controll...
bitnami/nginx-intel                     2.1.15          0.4.9           DEPRECATED NGINX Open Source for Intel is a lig...
ubuntu@ip-172-31-6-80:~$ helm search repo bitnami
NAME                                            CHART VERSION   APP VERSION     DESCRIPTION
bitnami/airflow                                 25.0.2          3.0.5           Apache Airflow is a tool to express and execute...
bitnami/apache                                  11.4.29         2.4.65          Apache HTTP Server is an open-source HTTP serve...
bitnami/apisix                                  6.0.0           3.13.0          Apache APISIX is high-performance, real-time AP...
bitnami/appsmith                                7.0.3           1.85.0          Appsmith is an open source platform for buildin...
bitnami/argo-cd                                 11.0.0          3.1.1           Argo CD is a continuous delivery tool for Kuber...
bitnami/argo-workflows                          13.0.6          3.7.1           Argo Workflows is meant to orchestrate Kubernet...
bitnami/aspnet-core                             9.4.23          10.0.12         ASP.NET Core is an open-source framework for we...
bitnami/cadvisor                                0.1.13          0.53.0          cAdvisor (Container Advisor) is an open-source ...
bitnami/cassandra                               12.3.11         5.0.5           Apache Cassandra is an open source distributed ...
bitnami/cert-manager                            1.5.14          1.18.2          cert-manager is a Kubernetes add-on to automate...
bitnami/chainloop                               4.0.75          1.43.1          Chainloop is an open-source Software Supply Cha...
bitnami/cilium                                  3.1.9           1.18.1          Cilium is an eBPF-based networking, observabili...
bitnami/clickhouse                              9.4.4           25.7.5          ClickHouse is an open-source column-oriented OL...
bitnami/clickhouse-operator                     0.2.33          0.25.3          ClickHouse Operator is a production-ready opera...
bitnami/cloudnative-pg                          1.0.11          1.26.1          CloudNativePG is an open-source tool for managi...
bitnami/common                                  2.41.0          2.41.0          A Library Helm Chart for grouping common logic ...
bitnami/concourse                               5.1.45          7.13.2          Concourse is an automation system written in Go...
bitnami/consul                                  11.4.32         1.21.4          HashiCorp Consul is a tool for discovering and ...
bitnami/contour                                 21.1.4          1.32.1          Contour is an open source Kubernetes ingress co...
bitnami/contour-operator                        4.2.1           1.24.0          DEPRECATED The Contour Operator extends the Kub...
bitnami/dataplatform-bp2                        12.0.5          1.0.1           DEPRECATED This Helm chart can be used for the ...
bitnami/deepspeed                               2.3.50          0.17.5          DeepSpeed is deep learning software suite for e...
bitnami/discourse                               17.0.1          3.5.0           Discourse is an open source discussion platform...
bitnami/dokuwiki                                16.2.11         20240206.1.0    DEPRECATED DokuWiki is a standards-compliant wi...
bitnami/dremio                                  3.0.13          26.0.0          Dremio is an open-source self-service data acce...
bitnami/drupal                                  23.0.0          11.2.3          Drupal is one of the most versatile open source...
bitnami/ejbca                                   19.0.0          9.1.1           EJBCA is an enterprise class PKI Certificate Au...
bitnami/elasticsearch                           22.1.6          9.1.2           Elasticsearch is a distributed search and analy...
bitnami/envoy-gateway                           2.0.4           1.5.0           Envoy Gateway simplifies traffic management by ...
bitnami/etcd                                    12.0.18         3.6.4           etcd is a distributed key-value store designed ...
bitnami/external-dns                            9.0.3           0.18.0          ExternalDNS is a Kubernetes addon that configur...
bitnami/flink                                   2.0.7           2.1.0           Apache Flink is a framework and distributed pro...
bitnami/fluent-bit                              3.1.13          4.0.8           Fluent Bit is a Fast and Lightweight Log Proces...
bitnami/fluentd                                 7.2.5           1.19.0          Fluentd collects events from various data sourc...
bitnami/flux                                    2.4.36          1.6.2           Source Controller is a component of Flux. Flux ...
bitnami/geode                                   1.1.8           1.15.1          DEPRECATED Apache Geode is a data management pl...
bitnami/ghost                                   25.0.4          6.0.5           Ghost is an open source publishing platform des...
bitnami/gitea                                   3.2.22          1.24.5          Gitea is a lightweight code hosting solution. W...
bitnami/gitlab-runner                           1.1.8           18.3.0          Gitlab Runner is an auxiliary application for G...
bitnami/grafana                                 12.1.8          12.1.1          Grafana is an open source metric analytics and ...
bitnami/grafana-alloy                           1.0.7           1.10.2          Grafana Alloy is an open source OpenTelemetry C...
bitnami/grafana-k6-operator                     1.0.11          0.0.23          Grafana k6 Operator is a Kubernetes operator th...
bitnami/grafana-loki                            6.0.6           3.5.3           Grafana Loki is a horizontally scalable, highly...
bitnami/grafana-mimir                           3.0.18          2.17.0          Grafana Mimir is an open source, horizontally s...
bitnami/grafana-operator                        4.9.37          5.19.4          Grafana Operator is a Kubernetes operator that ...
bitnami/grafana-tempo                           4.0.17          2.8.2           Grafana Tempo is a distributed tracing system t...
bitnami/haproxy                                 4.2.5           3.4.4           HAProxy is a TCP proxy and a HTTP reverse proxy...
bitnami/haproxy-intel                           0.2.11          2.7.1           DEPRECATED HAProxy for Intel is a high-performa...
bitnami/harbor                                  27.0.3          2.13.2          Harbor is an open source trusted cloud-native r...
bitnami/influxdb                                7.1.20          3.4.1           InfluxDB(TM) Core is an open source time-series...
bitnami/jaeger                                  6.0.5           2.9.0           Jaeger is a distributed tracing system. It is u...
bitnami/janusgraph                              1.4.10          1.1.0           JanusGraph is a scalable graph database optimiz...
bitnami/jasperreports                           18.2.5          8.2.0           DEPRECATED JasperReports Server is a stand-alon...
bitnami/jenkins                                 13.6.17         2.516.2         Jenkins is an open source Continuous Integratio...
bitnami/joomla                                  20.0.4          5.1.2           DEPRECATED Joomla! is an award winning open sou...
bitnami/jupyterhub                              10.0.5          5.3.0           JupyterHub brings the power of notebooks to gro...
bitnami/kafka                                   32.4.3          4.0.0           Apache Kafka is a distributed streaming platfor...
bitnami/keycloak                                25.2.0          26.3.3          Keycloak is a high performance Java-based ident...
bitnami/keydb                                   0.5.22          6.3.4           KeyDB is a high performance fork of Redis with ...
bitnami/kiam                                    2.3.16          4.2.0           kiam is a proxy that captures AWS Metadata API ...
bitnami/kibana                                  12.1.10         9.1.2           Kibana is an open source, browser based analyti...
bitnami/kong                                    15.4.22         3.9.1           Kong is an open source Microservice API gateway...
bitnami/kube-arangodb                           0.1.23          1.3.0           kube-arangodb is a Kubernetes Operator that man...
bitnami/kube-prometheus                         11.3.10         0.85.0          Prometheus Operator provides easy monitoring de...
bitnami/kube-state-metrics                      5.1.0           2.16.0          kube-state-metrics is a simple service that lis...
bitnami/kubeapps                                18.0.1          2.12.1          DEPRECATED Kubeapps is a web-based UI for launc...
bitnami/kuberay                                 1.4.29          1.4.2           KubeRay is a Kubernetes operator for deploying ...
bitnami/kubernetes-event-exporter               3.6.3           1.7.0           Kubernetes Event Exporter makes it easy to expo...
bitnami/logstash                                7.0.11          9.1.2           Logstash is an open source data processing engi...
bitnami/magento                                 28.0.5          2.4.7           DEPRECATED Magento is a powerful open source e-...
bitnami/mariadb                                 28.0.2          13.1.1          MariaDB is an open source, community-developed ...
bitnami/mariadb-galera                          16.0.1          12.0.2          MariaDB Galera is a multi-primary database clus...
bitnami/mastodon                                14.0.0          4.4.3           Mastodon is self-hosted social network server b...
bitnami/matomo                                  11.0.0          5.3.2           Matomo, formerly known as Piwik, is a real time...
bitnami/mediawiki                               21.0.5          1.42.1          DEPRECATED MediaWiki is the free and open sourc...
bitnami/memcached                               8.8.1           1.6.45          Memcached is an high-performance, distributed m...
bitnami/metallb                                 6.4.22          0.15.2          MetalLB is a load-balancer implementation for b...
bitnami/metrics-server                          7.4.12          0.8.0           Metrics Server aggregates resource usage data, ...
bitnami/milvus                                  16.0.1          2.6.0           Milvus is a cloud-native, open-source vector da...
bitnami/minio                                   17.0.21         2025.7.23       MinIO(R) is an object storage server, compatibl...
bitnami/minio-operator                          0.2.9           7.1.1           MinIO(R) Operator is a Kubernetes-native tool f...
bitnami/mlflow                                  5.1.17          3.3.2           MLflow is an open-source platform designed to m...
bitnami/mongodb                                 19.2.1          8.3.11          MongoDB(R) is a relational open source NoSQL da...
bitnami/mongodb-sharded                         9.4.12          8.0.13          MongoDB(R) is an open source NoSQL database tha...
bitnami/moodle                                  28.0.0          5.0.2           Moodle(TM) LMS is an open source online Learnin...
bitnami/multus-cni                              2.2.21          4.2.2           Multus is a CNI plugin for Kubernetes clusters....
bitnami/mxnet                                   3.5.2           1.9.1           DEPRECATED Apache MXNet (Incubating) is a flexi...
bitnami/mysql                                   14.0.3          9.4.0           MySQL is a fast, reliable, scalable, and easy t...
bitnami/nats                                    9.0.28          2.11.8          NATS is an open source, lightweight and high-pe...
bitnami/neo4j                                   0.4.14          5.26.11         Neo4j is a high performance graph store with al...
bitnami/nessie                                  2.0.34          0.104.10        Nessie is an open-source version control system...
bitnami/nginx                                   25.1.14         1.31.6          NGINX Open Source is a web server that can be a...
bitnami/nginx-ingress-controller                12.0.7          1.13.1          NGINX Ingress Controller is an Ingress controll...
bitnami/nginx-intel                             2.1.15          0.4.9           DEPRECATED NGINX Open Source for Intel is a lig...
bitnami/node                                    19.1.7          16.18.0         DEPRECATED Node.js is a runtime environment bui...
bitnami/node-exporter                           4.5.19          1.9.1           Prometheus exporter for hardware and OS metrics...
bitnami/oauth2-proxy                            8.0.2           7.12.0          A reverse proxy and static file server that pro...
bitnami/odoo                                    28.2.10         18.0.20250805   Odoo is an open source ERP and CRM platform, fo...
bitnami/opencart                                19.0.4          4.0.2-3         DEPRECATED OpenCart is free open source ecommer...
bitnami/opensearch                              2.0.10          3.2.0           OpenSearch is a scalable open-source solution f...
bitnami/osclass                                 18.2.6          8.2.0           DEPRECATED Osclass allows you to easily create ...
bitnami/owncloud                                12.2.11         10.11.0         DEPRECATED ownCloud is an open source content c...
bitnami/parse                                   25.1.15         8.2.3           Parse is a platform that enables users to add a...
bitnami/phpbb                                   19.0.4          3.3.12          DEPRECATED phpBB is a popular bulletin board th...
bitnami/phpmyadmin                              20.0.0          5.2.2           phpMyAdmin is a free software tool written in P...
bitnami/pinniped                                2.4.23          0.40.0          Pinniped is an identity service provider for Ku...
bitnami/postgresql                              18.11.6         18.6.0          PostgreSQL (Postgres) is an open source object-...
bitnami/postgresql-ha                           16.3.2          17.6.0          This PostgreSQL cluster solution includes the P...
bitnami/prestashop                              22.0.5          8.1.7           DEPRECATED PrestaShop is a powerful open source...
bitnami/prometheus                              2.1.23          3.5.0           Prometheus is an open source monitoring and ale...
bitnami/pytorch                                 5.9.23          2.14.0          PyTorch is a deep learning platform that accele...
bitnami/rabbitmq                                16.0.14         4.1.3           RabbitMQ is an open source general-purpose mess...
bitnami/rabbitmq-cluster-operator               4.4.34          2.16.1          The RabbitMQ Cluster Kubernetes Operator automa...
bitnami/redis                                   28.2.3          8.10.2          Redis(R) is an open source, advanced key-value ...
bitnami/redis-cluster                           13.0.4          8.2.1           Redis(R) is an open source, scalable, distribut...
bitnami/redmine                                 34.0.0          6.0.6           Redmine is an open source management applicatio...
bitnami/schema-registry                         26.0.5          8.0.0           Confluent Schema Registry provides a RESTful in...
bitnami/scylladb                                5.0.6           2025.2.2        ScyllaDB is an open-source, distributed NoSQL w...
bitnami/sealed-secrets                          2.5.19          0.31.0          Sealed Secrets are "one-way" encrypted K8s Secr...
bitnami/seaweedfs                               6.0.1           3.96.0          SeaweedFS is a simple and highly scalable distr...
bitnami/solr                                    9.6.10          9.9.0           Apache Solr is an extremely powerful, open sour...
bitnami/sonarqube                               8.1.17          25.8.0          SonarQube(TM) is an open source quality managem...
bitnami/spark                                   10.0.3          4.0.0           Apache Spark is a high-performance engine for l...
bitnami/spring-cloud-dataflow                   37.0.5          2.11.5          DEPRECATED Spring Cloud Data Flow is a microser...
bitnami/suitecrm                                14.1.1          7.13.4          DEPRECATED SuiteCRM is a completely open source...
bitnami/supabase                                5.3.6           1.24.7          DEPRECATED Supabase is an open source Firebase ...
bitnami/superset                                5.0.0           5.0.0           Superset is a modern data exploration and data ...
bitnami/tensorflow-resnet                       4.3.14          2.19.1          TensorFlow ResNet is a client utility for use w...
bitnami/thanos                                  17.3.1          0.39.2          Thanos is a highly available metrics system tha...
bitnami/tomcat                                  13.6.28         11.0.26         Apache Tomcat is an open-source web server desi...
bitnami/valkey                                  6.3.2           9.1.2           Valkey is an open source (BSD) high-performance...
bitnami/valkey-cluster                          3.0.24          8.1.3           Valkey is an open source (BSD) high-performance...
bitnami/vault                                   1.9.0           1.20.2          Vault is a tool for securely managing and acces...
bitnami/victoriametrics                         0.1.31          1.124.0         VictoriaMetrics is a fast, cost-effective, and ...
bitnami/wavefront                               4.4.3           1.13.0          DEPRECATED Wavefront is a high-performance stre...
bitnami/wavefront-adapter-for-istio             2.0.6           0.1.5           DEPRECATED Wavefront Adapter for Istio is an ad...
bitnami/wavefront-hpa-adapter                   1.5.2           0.9.10          DEPRECATED Wavefront HPA Adapter for Kubernetes...
bitnami/wavefront-prometheus-storage-adapter    2.3.3           1.0.7           DEPRECATED Wavefront Storage Adapter is a Prome...
bitnami/whereabouts                             1.2.19          0.9.2           Whereabouts is a CNI IPAM plugin for Kubernetes...
bitnami/wildfly                                 25.0.0          37.0.0          Wildfly is a lightweight, open source applicati...
bitnami/wordpress                               34.0.3          7.1.1           WordPress is the world's most popular blogging ...
bitnami/wordpress-intel                         2.1.31          6.1.1           DEPRECATED WordPress for Intel is the most popu...
bitnami/zipkin                                  1.3.11          3.5.1           Zipkin is a distributed tracing system that hel...
bitnami/zookeeper                               13.8.7          3.9.3           Apache ZooKeeper provides a reliable, centraliz...
ubuntu@ip-172-31-6-80:~$

```

### Verify: How many charts does Bitnami have?

Result: Bitnami has 144 charts available in the configured repository.

## Task 3: Install a Chart

### Deploy NGINX using helm install my-nginx bitnami/nginx, check the created resources with kubectl get all, and inspect the release using helm list, helm status my-nginx, and helm get manifest my-nginx.

```bash
ubuntu@ip-172-31-6-80:~$ helm install my-nginx bitnami/nginx
NAME: my-nginx
LAST DEPLOYED: Tue Sep 22 09:55:16 2026
NAMESPACE: default
STATUS: deployed
REVISION: 1
DESCRIPTION: Install complete
TEST SUITE: None
NOTES:
CHART NAME: nginx
CHART VERSION: 25.1.14
APP VERSION: 1.31.6

⚠ WARNING: Since August 28th, 2025, only a limited subset of images/charts are available for free.
    Subscribe to Bitnami Secure Images to receive continued support and security updates.
    More info at https://bitnami.com and https://github.com/bitnami/containers/issues/83267

** Please be patient while the chart is being deployed **
NGINX can be accessed through the following DNS name from within your cluster:

    my-nginx.default.svc.cluster.local (port 80)

To access NGINX from outside the cluster, follow the steps below:

1. Get the NGINX URL by running these commands:

  NOTE: It may take a few minutes for the LoadBalancer IP to be available.
        Watch the status with: 'kubectl get svc --namespace default -w my-nginx'

    export SERVICE_PORT=$(kubectl get --namespace default -o jsonpath="{.spec.ports[0].port}" services my-nginx)
    export SERVICE_IP=$(kubectl get svc --namespace default my-nginx -o jsonpath='{.status.loadBalancer.ingress[0].ip}')
    echo "http://${SERVICE_IP}:${SERVICE_PORT}"
WARNING: Rolling tag detected (bitnami/nginx:latest), please note that it is strongly recommended to avoid using rolling tags in a production environment.
+info https://techdocs.broadcom.com/us/en/vmware-tanzu/bitnami-secure-images/bitnami-secure-images/services/bsi-doc/apps-tutorials-understand-rolling-tags-containers-index.html
WARNING: Rolling tag detected (bitnami/git:latest), please note that it is strongly recommended to avoid using rolling tags in a production environment.
+info https://techdocs.broadcom.com/us/en/vmware-tanzu/bitnami-secure-images/bitnami-secure-images/services/bsi-doc/apps-tutorials-understand-rolling-tags-containers-index.html
WARNING: Rolling tag detected (bitnami/nginx-exporter:latest), please note that it is strongly recommended to avoid using rolling tags in a production environment.
+info https://techdocs.broadcom.com/us/en/vmware-tanzu/bitnami-secure-images/bitnami-secure-images/services/bsi-doc/apps-tutorials-understand-rolling-tags-containers-index.html

WARNING: There are "resources" sections in the chart not set. Using "resourcesPreset" is not recommended for production. For production installations, please set the following values according to your workload needs:
  - cloneStaticSiteFromGit.gitSync.resources
  - resources
+info https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/
ubuntu@ip-172-31-6-80:~$
ubuntu@ip-172-31-6-80:~$
ubuntu@ip-172-31-6-80:~$ kubectl get all
NAME                            READY   STATUS    RESTARTS   AGE
pod/my-nginx-54c75f846f-lbrvp   1/1     Running   0          15s

NAME                 TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)                      AGE
service/kubernetes   ClusterIP      10.96.0.1       <none>        443/TCP                      3d14h
service/my-nginx     LoadBalancer   10.96.230.217   <pending>     80:30277/TCP,443:32060/TCP   15s

NAME                       READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/my-nginx   1/1     1            1           15s

NAME                                  DESIRED   CURRENT   READY   AGE
replicaset.apps/my-nginx-54c75f846f   1         1         1       15s
ubuntu@ip-172-31-6-80:~$

```
```bash
ubuntu@ip-172-31-6-80:~$ helm list
NAME            NAMESPACE       REVISION        UPDATED                                 STATUS          CHART           APP VERSION
my-nginx        default         1               2026-09-22 09:55:16.183588943 +0000 UTC deployed        nginx-25.1.14   1.31.6
ubuntu@ip-172-31-6-80:~$ helm status my-nginx
NAME: my-nginx
LAST DEPLOYED: Tue Sep 22 09:55:16 2026
NAMESPACE: default
STATUS: deployed
REVISION: 1
DESCRIPTION: Install complete
RESOURCES:
==> v1/Service
NAME       TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)                      AGE
my-nginx   LoadBalancer   10.96.230.217   <pending>     80:30277/TCP,443:32060/TCP   9m55s

==> v1/Deployment
NAME       READY   UP-TO-DATE   AVAILABLE   AGE
my-nginx   1/1     1            1           9m55s

==> v1/Pod(related)
NAME                        READY   STATUS    RESTARTS   AGE
my-nginx-54c75f846f-lbrvp   1/1     Running   0          9m55s

==> v1/NetworkPolicy
NAME       POD-SELECTOR                                                       AGE
my-nginx   app.kubernetes.io/instance=my-nginx,app.kubernetes.io/name=nginx   9m55s

==> v1/PodDisruptionBudget
NAME       MIN AVAILABLE   MAX UNAVAILABLE   ALLOWED DISRUPTIONS   AGE
my-nginx   N/A             1                 1                     9m55s

==> v1/ServiceAccount
NAME       AGE
my-nginx   9m55s

==> v1/Secret
NAME           TYPE                DATA   AGE
my-nginx-tls   kubernetes.io/tls   3      9m55s


TEST SUITE: None
NOTES:
CHART NAME: nginx
CHART VERSION: 25.1.14
APP VERSION: 1.31.6

⚠ WARNING: Since August 28th, 2025, only a limited subset of images/charts are available for free.
    Subscribe to Bitnami Secure Images to receive continued support and security updates.
    More info at https://bitnami.com and https://github.com/bitnami/containers/issues/83267

** Please be patient while the chart is being deployed **
NGINX can be accessed through the following DNS name from within your cluster:

    my-nginx.default.svc.cluster.local (port 80)

To access NGINX from outside the cluster, follow the steps below:

1. Get the NGINX URL by running these commands:

  NOTE: It may take a few minutes for the LoadBalancer IP to be available.
        Watch the status with: 'kubectl get svc --namespace default -w my-nginx'

    export SERVICE_PORT=$(kubectl get --namespace default -o jsonpath="{.spec.ports[0].port}" services my-nginx)
    export SERVICE_IP=$(kubectl get svc --namespace default my-nginx -o jsonpath='{.status.loadBalancer.ingress[0].ip}')
    echo "http://${SERVICE_IP}:${SERVICE_PORT}"
WARNING: Rolling tag detected (bitnami/nginx:latest), please note that it is strongly recommended to avoid using rolling tags in a production environment.
+info https://techdocs.broadcom.com/us/en/vmware-tanzu/bitnami-secure-images/bitnami-secure-images/services/bsi-doc/apps-tutorials-understand-rolling-tags-containers-index.html
WARNING: Rolling tag detected (bitnami/git:latest), please note that it is strongly recommended to avoid using rolling tags in a production environment.
+info https://techdocs.broadcom.com/us/en/vmware-tanzu/bitnami-secure-images/bitnami-secure-images/services/bsi-doc/apps-tutorials-understand-rolling-tags-containers-index.html
WARNING: Rolling tag detected (bitnami/nginx-exporter:latest), please note that it is strongly recommended to avoid using rolling tags in a production environment.
+info https://techdocs.broadcom.com/us/en/vmware-tanzu/bitnami-secure-images/bitnami-secure-images/services/bsi-doc/apps-tutorials-understand-rolling-tags-containers-index.html

WARNING: There are "resources" sections in the chart not set. Using "resourcesPreset" is not recommended for production. For production installations, please set the following values according to your workload needs:
  - cloneStaticSiteFromGit.gitSync.resources
  - resources
+info https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/
ubuntu@ip-172-31-6-80:~$
ubuntu@ip-172-31-6-80:~$
ubuntu@ip-172-31-6-80:~$
ubuntu@ip-172-31-6-80:~$ helm get manifest my-nginx
---
# Source: nginx/templates/networkpolicy.yaml
kind: NetworkPolicy
apiVersion: networking.k8s.io/v1
metadata:
  name: my-nginx
  namespace: "default"
  labels:
    app.kubernetes.io/instance: my-nginx
    app.kubernetes.io/managed-by: Helm
    app.kubernetes.io/name: nginx
    app.kubernetes.io/version: 1.31.6
    helm.sh/chart: nginx-25.1.14
spec:
  podSelector:
    matchLabels:
      app.kubernetes.io/instance: my-nginx
      app.kubernetes.io/name: nginx
  policyTypes:
    - Ingress
    - Egress
  egress:
    - {}
  ingress:
    - ports:
        - port: 8080
        - port: 8443

---
# Source: nginx/templates/pdb.yaml
apiVersion: policy/v1
kind: PodDisruptionBudget
metadata:
  name: my-nginx
  namespace: "default"
  labels:
    app.kubernetes.io/instance: my-nginx
    app.kubernetes.io/managed-by: Helm
    app.kubernetes.io/name: nginx
    app.kubernetes.io/version: 1.31.6
    helm.sh/chart: nginx-25.1.14
spec:
  maxUnavailable: 1
  selector:
    matchLabels:
      app.kubernetes.io/instance: my-nginx
      app.kubernetes.io/name: nginx

---
# Source: nginx/templates/serviceaccount.yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: my-nginx
  namespace: "default"
  labels:
    app.kubernetes.io/instance: my-nginx
    app.kubernetes.io/managed-by: Helm
    app.kubernetes.io/name: nginx
    app.kubernetes.io/version: 1.31.6
    helm.sh/chart: nginx-25.1.14
automountServiceAccountToken: false
---
# Source: nginx/templates/tls-secret.yaml
apiVersion: v1
kind: Secret
metadata:
  name: my-nginx-tls
  namespace: "default"
  labels:
    app.kubernetes.io/instance: my-nginx
    app.kubernetes.io/managed-by: Helm
    app.kubernetes.io/name: nginx
    app.kubernetes.io/version: 1.31.6
    helm.sh/chart: nginx-25.1.14
type: kubernetes.io/tls
data:
  tls.crt: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSURjekNDQWx1Z0F3SUJBZ0lRTnFJc1JzaVRDQm5kaS8yVGZBYjd5REFOQmdrcWhraUc5dzBCQVFzRkFEQVQKTVJFd0R3WURWUVFERXdodVoybHVlQzFqWVRBZUZ3MHlOakE1TWpJd09UVTFNVFphRncweU56QTVNakl3T1RVMQpNVFphTUJNeEVUQVBCZ05WQkFNVENHMTVMVzVuYVc1NE1JSUJJakFOQmdrcWhraUc5dzBCQVFFRkFBT0NBUThBCk1JSUJDZ0tDQVFFQXdVMngxbE5BYWNQdHZpZTBobWVVaHZPdWQ1dXNZejhHMUMyaUVGTDdPVmV3NHRSSm4yRFUKbEhIVjJ5c1l4YVNJTCtJMCtFWitSUEZvVFNHRWgwczBULzhlZ01zT1pLUS82UXQ2UlErTi9YL09xUzFLalJwMwpqUTJaNVdQbXpVNXE5WmZrd1VhMzJNeTh1TnRFMDNUbjRHYnRJa1liT3JzN0xhZTVqYWpTU0JGK0liTG1VUWdOCi8za2pYNXBLR2llV3pYU3BNM2c1VWJSMFJNZi9uMmloNERWSkpQNEJYZFUzWWxJek5VNjYraEc5dmZKbms1VFAKSC9nWDRTUzNRSTZWeGlpR09mOVlyVlMraThYSmpoa0pLL2owdmI3YS81ZHQwamVQb3RMODBUWXFoaWlJRGErYQo4ZEpYV1p3cXN3LzlsSHFmVjlubzVEL3dtdDNEbUdMM2tRSURBUUFCbzRIQ01JRy9NQTRHQTFVZER3RUIvd1FFCkF3SUZvREFkQmdOVkhTVUVGakFVQmdnckJnRUZCUWNEQVFZSUt3WUJCUVVIQXdJd0RBWURWUjBUQVFIL0JBSXcKQURBZkJnTlZIU01FR0RBV2dCU05rU3RVSzh4NVZSMXRMcGJ2V2EzdXdlenlKVEJmQmdOVkhSRUVXREJXZ2dodAplUzF1WjJsdWVJSVFiWGt0Ym1kcGJuZ3VaR1ZtWVhWc2RJSVViWGt0Ym1kcGJuZ3VaR1ZtWVhWc2RDNXpkbU9DCkltMTVMVzVuYVc1NExtUmxabUYxYkhRdWMzWmpMbU5zZFhOMFpYSXViRzlqWVd3d0RRWUpLb1pJaHZjTkFRRUwKQlFBRGdnRUJBTHpSTUVHT2JRcys2NXdJQmJLWEJ1dVhHVnVLU1VuTUh3TlFYa2RHeE5ibDVFcEpLNVpYSkNwRApJMjNZY0FSV0NqNzNsbnlWczZyaVYrMVZ5VkszQnVhaC9KbWJpN1dqcFdKYUF0RjJLRSs0WlBSd2RkRDRtNmJLCmtBU3k4ZjlQTU9YZmZoV3lqajBYTStuYXVUU2hOQS9tMllBZkw2Zzc0SkxUTG1oMWJhUEI2VzBsTWJRK1RpWkEKdDFoaU04Qy9scnU3QWhaZytqV2V6WWtKM2trQkJjWlF4bW1iM2VPVWExNG16c3RjY1B5SDdyekRxVmd6bDhYaApLWkJBK0kwUFIxWkxla2d5L2VreWY2bTZUMDhYSmUxY0xpaG5MQVhoQlc3RkNlRTZmQVdKMG5oZTcrTWFVamRPClVPQ2t1SURMdUdneHNwRE9ieTVoSlhTUjZzRGZjbmM9Ci0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K
  tls.key: LS0tLS1CRUdJTiBSU0EgUFJJVkFURSBLRVktLS0tLQpNSUlFb3dJQkFBS0NBUUVBd1UyeDFsTkFhY1B0dmllMGhtZVVodk91ZDV1c1l6OEcxQzJpRUZMN09WZXc0dFJKCm4yRFVsSEhWMnlzWXhhU0lMK0kwK0VaK1JQRm9UU0dFaDBzMFQvOGVnTXNPWktRLzZRdDZSUStOL1gvT3FTMUsKalJwM2pRMlo1V1BtelU1cTlaZmt3VWEzMk15OHVOdEUwM1RuNEdidElrWWJPcnM3TGFlNWphalNTQkYrSWJMbQpVUWdOLzNralg1cEtHaWVXelhTcE0zZzVVYlIwUk1mL24yaWg0RFZKSlA0QlhkVTNZbEl6TlU2NitoRzl2ZkpuCms1VFBIL2dYNFNTM1FJNlZ4aWlHT2Y5WXJWUytpOFhKamhrSksvajB2YjdhLzVkdDBqZVBvdEw4MFRZcWhpaUkKRGErYThkSlhXWndxc3cvOWxIcWZWOW5vNUQvd210M0RtR0wza1FJREFRQUJBb0lCQUFTeldLSzhtREZrNC9ySgpjN2l5VU1palU2MGNQNm5vMWgxWGFyeDQ0c1poM1M3MjR6OXVsd2R4RStFN2dBNWcvT0FsTm1LSjNFemRqZlZxCk01TW4vMWYrcDlybWxTMWFBbnFEOCsrS3B2VHd6d2ExZWoyZmYzbG5ua1FYYWllNG1NaVdHVkwxM3ZIMUJlc24KaHk5RDJ0Wk55bkJ0N0tyclNLVElrbGpoTDc2SmhqU3ovTzUwcTAvdVdDNm1ta0g3OHB6TjBoNldRS2NhSzBWMwpNN0c5aU9MMyt2dXNHZDZ6cno0RWkrU1RvZm1TYm5DVWZ3UVl2c2w5cWx5d0F0WE96SnVZMGtXN3poUlQySnk3ClB0NC8rYXBiMTlhTjhoR1JxN00xTE1nMHplS0Q2K3RYdlgrQkhkditLbnI4MHQrRTEzQm15YVVITGRid2VuMHkKZmNzclN4MENnWUVBNGMyUWFhbGdEUFFFZG1RTjZHZzlPNlZDdFdoTkFxTldwWnlwdkE1NFVRVDdMT2RPR1p0OQoxVUZ5a1B4VXhxWXowckZCRzlzWitRdG1QWGcvam1reUZHNlFjeWlqc2VCVHYyV1UvQVo2a3BCc2FhbTBBSmZZCkFyZHZXVHg2NzhEa1Y4L1NFa1BncDVPYTF1YVI4dExvY0cvVk9JN0JrSmJ5U2ZTM01MZ0sxazhDZ1lFQTJ5ZUEKRnRqN2FVN2U4ZVZxU2Jnelp6RG9MZURaTW9CSzVqakdiSW1lcW1rSlNPWmxFYW5JTmxLWTkrQVBjYzRMcE5tUgpMaEdDckN1RmdnRFpxNkRBRmhTeldaclprUXBGaTFoVTlPbTlOajcrbmR6RHpqaXJkTkRiSzA3bmJrQXFxa3Z4CnhGcC9BSzduemN3eWoydXZrQ2Y3cDlHd3owSzBMTWRNWU0vL3ZCOENnWUF5Vm5ORk90OFF4QzFpZnplaWdlcDAKcTRqTmpDenUwNTd6V0pOMk92dVRoRHJDYmVZNVN6S29JZWo2YldZd3lzaHV4ZGt2N281QnVNcllGVUNGN09tZgpLRzdIWFYzd3Y0T3IvV2RUTDlhUGFlYmhQMVhEZEJaUnRMYjcrOEdrUlNvaWNVL3hoblJFcDJFeld6OWFGSzZBCnNrMmtTQjdhcnV2Z2xNOXA2djF2ZlFLQmdRRE4zbUZaRlNPM1hUdlppR1U4TXlrMmVwN2cyaU91YVEzekRzcDMKRXlCVmZLNFlLVFl3VFltaVhoME1YUktsR2FXZWlqTHpUOGVzN0lWU0JuSno0MklPWEF2TzFNUWtsNzJVbExuYQpCK2lTbU1LZWtNL3ZYUlRUZTQ4bk04djdxWk5xdmtTeTZ6LzY2Rk1nNC8xcTlRSExMWVFkdGNHZU1VOEg4WUF5ClFiSStpUUtCZ0I3NU1mMjMySEE3ZFp1TzExelFXWXltS3AyNmFmcXg2clFaQXV2M1daaDFUYkdsd3FEQ1B6dEIKcGVHa284VVRwWUhZdm8xQkhXT0V2bE9nd2F5MEdkQmpXclR5K21JRjQrRHVpcTI4aThTYXVxZ3I3NjhwVXFCRAp3RDJjQk4rWjZzU2hPYkxLMzBWcWZpamMrQ2FYeVJ6VytJakpCYnNQczZqYktOMlMrSkZhCi0tLS0tRU5EIFJTQSBQUklWQVRFIEtFWS0tLS0tCg==
  ca.crt: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSURFakNDQWZxZ0F3SUJBZ0lSQU5BUkVHSGR6c3FPR0tnWThuUk45am93RFFZSktvWklodmNOQVFFTEJRQXcKRXpFUk1BOEdBMVVFQXhNSWJtZHBibmd0WTJFd0hoY05Nall3T1RJeU1EazFOVEUyV2hjTk1qY3dPVEl5TURrMQpOVEUyV2pBVE1SRXdEd1lEVlFRREV3aHVaMmx1ZUMxallUQ0NBU0l3RFFZSktvWklodmNOQVFFQkJRQURnZ0VQCkFEQ0NBUW9DZ2dFQkFORkcxalNISm9tYmV3amg0Nmxja0s2bk12MEhnd1FjbmdYa0NhRE51dXNjeTBoV0Fla0kKNWVUM3FxWkRJcUwvQ1MvME82Y0JFUU5zcm5ZbTBBdWwvV1VCS1B2cGwwaEd1MWV1czdnOFNpVWVob3lNMWZHTwpKQVVRTHhPY1JhM1N1VC9YOW9kR2J6alR3emI2L2JxMlRYeklQZy9RU1ZLdzViMXU0V3NWV1hwbzY1Z01ScmhNCjhJZHcrRkZIVnRYOUdUTnFlbGRDWWpxdWliYklPWG9TWmFwRE85UExTQ1RmN0crMTFPVEhON1VSWk5tbWhzVmQKTGtsN2NWKzVJZzRCLzlJM3ZxTC9icmVnUzBMaDdaVDdRUkkweXhRTEx4ekoxMjhLbnZhUS95djBQQldWd3pyMgpaVVMyME5ISEJyMHVuR1RrWVhZRHJLRWV5TEZRYUFWZmU1a0NBd0VBQWFOaE1GOHdEZ1lEVlIwUEFRSC9CQVFECkFnS2tNQjBHQTFVZEpRUVdNQlFHQ0NzR0FRVUZCd01CQmdnckJnRUZCUWNEQWpBUEJnTlZIUk1CQWY4RUJUQUQKQVFIL01CMEdBMVVkRGdRV0JCU05rU3RVSzh4NVZSMXRMcGJ2V2EzdXdlenlKVEFOQmdrcWhraUc5dzBCQVFzRgpBQU9DQVFFQUZrZlZCTjAzZjl3MWp5Q09XNXZPeU5XaFdNUkhNMGdidENDZy96RExrd2MyRWJybGNTOE0vU3VCCi83N2pWNDNjNVJ6Mjdxd3NiaGwrbmVmbmY2ekVoK1Jscmcyb050aE1EaFlRbFpuR2w2dkhwdisvdUc5YjhkU1UKRm5xVExVd1VFMjVQUFVEUHYraTcyQVZHYitSS0lQTzU5ZVBHREpyeFdkSldNUXhWZ0tuRVpZcnFpalZVSmJicgpGWElJZDVIVnJwWXVlbzJoZkczcmUzQUU5VWRPaGUvVUQ0TlNzUEhxRmRFTUFsY1AvcG1vYXdvT0VwTzQxVkYzCkNsKzdteUlybG82UFVuMlRsbE5yOGp2SFd2NCtZaEw3K040bzZPYmhhZHVtc3p5bkNCQzFIdGZJTmhmNnZsTE0KMFpSdFRRZmovQlNQeDdkQXphU3haY1ZWNFFiajZRPT0KLS0tLS1FTkQgQ0VSVElGSUNBVEUtLS0tLQo=

---
# Source: nginx/templates/svc.yaml
apiVersion: v1
kind: Service
metadata:
  name: my-nginx
  namespace: "default"
  labels:
    app.kubernetes.io/instance: my-nginx
    app.kubernetes.io/managed-by: Helm
    app.kubernetes.io/name: nginx
    app.kubernetes.io/version: 1.31.6
    helm.sh/chart: nginx-25.1.14
  annotations:
spec:
  type: LoadBalancer
  externalTrafficPolicy: "Cluster"
  ports:
    - name: http
      port: 80
      targetPort: http
    - name: https
      port: 443
      targetPort: https
  selector:
    app.kubernetes.io/instance: my-nginx
    app.kubernetes.io/name: nginx

---
# Source: nginx/templates/deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-nginx
  namespace: "default"
  labels:
    app.kubernetes.io/instance: my-nginx
    app.kubernetes.io/managed-by: Helm
    app.kubernetes.io/name: nginx
    app.kubernetes.io/version: 1.31.6
    helm.sh/chart: nginx-25.1.14
spec:
  replicas: 1
  revisionHistoryLimit: 10
  strategy:
    rollingUpdate: {}
    type: RollingUpdate
  selector:
    matchLabels:
      app.kubernetes.io/instance: my-nginx
      app.kubernetes.io/name: nginx
  template:
    metadata:
      labels:
        app.kubernetes.io/instance: my-nginx
        app.kubernetes.io/managed-by: Helm
        app.kubernetes.io/name: nginx
        app.kubernetes.io/version: 1.31.6
        helm.sh/chart: nginx-25.1.14
      annotations:
    spec:

      shareProcessNamespace: false
      serviceAccountName: my-nginx
      automountServiceAccountToken: false
      affinity:
        podAffinity:

        podAntiAffinity:
          preferredDuringSchedulingIgnoredDuringExecution:
            - podAffinityTerm:
                labelSelector:
                  matchLabels:
                    app.kubernetes.io/instance: my-nginx
                    app.kubernetes.io/name: nginx
                topologyKey: kubernetes.io/hostname
              weight: 1
        nodeAffinity:

      hostNetwork: false
      hostIPC: false
      securityContext:
        fsGroup: 1001
        fsGroupChangePolicy: Always
        supplementalGroups: []
        sysctls: []
      initContainers:
        - name: preserve-logs-symlinks
          image: registry-1.docker.io/bitnami/nginx:latest
          imagePullPolicy: "IfNotPresent"
          securityContext:
            allowPrivilegeEscalation: false
            capabilities:
              drop:
              - ALL
            privileged: false
            readOnlyRootFilesystem: true
            runAsGroup: 1001
            runAsNonRoot: true
            runAsUser: 1001
            seLinuxOptions: {}
            seccompProfile:
              type: RuntimeDefault
          resources:
            limits:
              cpu: 150m
              ephemeral-storage: 2Gi
              memory: 192Mi
            requests:
              cpu: 100m
              ephemeral-storage: 50Mi
              memory: 128Mi
          env:
            - name: OPENSSL_FIPS
              value: "yes"
          command:
            - /bin/bash
          args:
            - -ec
            - |
              #!/bin/bash
              . /opt/bitnami/scripts/libfs.sh
              # We copy the logs folder because it has symlinks to stdout and stderr
              if ! is_dir_empty /opt/bitnami/nginx/logs; then
                cp -r /opt/bitnami/nginx/logs /emptydir/app-logs-dir
              fi
          volumeMounts:
            - name: empty-dir
              mountPath: /emptydir
      containers:
        - name: nginx
          image: registry-1.docker.io/bitnami/nginx:latest
          imagePullPolicy: "IfNotPresent"
          securityContext:
            allowPrivilegeEscalation: false
            capabilities:
              drop:
              - ALL
            privileged: false
            readOnlyRootFilesystem: true
            runAsGroup: 1001
            runAsNonRoot: true
            runAsUser: 1001
            seLinuxOptions: {}
            seccompProfile:
              type: RuntimeDefault
          env:
            - name: BITNAMI_DEBUG
              value: "false"
            - name: NGINX_HTTP_PORT_NUMBER
              value: "8080"
            - name: OPENSSL_FIPS
              value: "yes"
            - name: NGINX_HTTPS_PORT_NUMBER
              value: "8443"
          envFrom:
          ports:
            - name: http
              containerPort: 8080
            - name: https
              containerPort: 8443
          livenessProbe:
            failureThreshold: 6
            initialDelaySeconds: 30
            periodSeconds: 10
            successThreshold: 1
            timeoutSeconds: 5
            tcpSocket:
              port: http
          readinessProbe:
            failureThreshold: 3
            initialDelaySeconds: 5
            periodSeconds: 5
            successThreshold: 1
            timeoutSeconds: 3
            httpGet:
              path: /
              port: http
          resources:
            limits:
              cpu: 150m
              ephemeral-storage: 2Gi
              memory: 192Mi
            requests:
              cpu: 100m
              ephemeral-storage: 50Mi
              memory: 128Mi
          volumeMounts:
            - name: empty-dir
              mountPath: /tmp
              subPath: tmp-dir
            - name: empty-dir
              mountPath: /opt/bitnami/nginx/conf
              subPath: app-conf-dir
            - name: empty-dir
              mountPath: /opt/bitnami/nginx/logs
              subPath: app-logs-dir
            - name: empty-dir
              mountPath: /opt/bitnami/nginx/tmp
              subPath: app-tmp-dir
            - name: certificate
              mountPath: /certs
      volumes:
        - name: empty-dir
          emptyDir: {}
        - name: certificate
          secret:
            secretName: my-nginx-tls
            items:
              - key: tls.crt
                path: tls.crt
              - key: tls.key
                path: tls.key


ubuntu@ip-172-31-6-80:~$
```
**Verify:** 1 Pod is running, and the `my-nginx` Service was created with the `LoadBalancer` type.

## Task 4: Customize with Values

### View the defaults with helm show values bitnami/nginx, then install a custom release using --set replicaCount=3 --set service.type=NodePort.

```bash

ubuntu@ip-172-31-6-80:~$ helm show values bitnami/nginx
# Copyright Broadcom, Inc. All Rights Reserved.
# SPDX-License-Identifier: APACHE-2.0

## @section Global parameters
## Global Docker image parameters
## Please, note that this will override the image parameters, including dependencies, configured to use the global value
## Current available global Docker image parameters: imageRegistry, imagePullSecrets and storageClass

## @param global.imageRegistry Global Docker image registry
## @param global.imagePullSecrets Global Docker registry secret names as an array
## @param global.defaultFips Default value for the FIPS configuration (allowed values: '', restricted, relaxed, off). Can be overridden by the 'fips' object
##
global:
  imageRegistry: ""
  ## E.g.
  ## imagePullSecrets:
  ##   - myRegistryKeySecretName
  ##
  imagePullSecrets: []
  ## Security parameters
  ##
  security:
    ## @param global.security.allowInsecureImages Allows skipping image verification
    ##
    allowInsecureImages: false
  ## Compatibility adaptations for Kubernetes platforms
  ##
  compatibility:
    ## Compatibility adaptations for Openshift
    ##
    openshift:
      ## @param global.compatibility.openshift.adaptSecurityContext Adapt the securityContext sections of the deployment to make them compatible with Openshift restricted-v2 SCC: remove runAsUser, runAsGroup and fsGroup and let the platform use their allowed default IDs. Possible values: auto (apply if the detected running cluster is Openshift), force (perform the adaptation always), disabled (do not perform adaptation)
      ##
      adaptSecurityContext: auto
  ##  Configure FIPS mode: '', 'restricted', 'relaxed', 'off'
  ##
  defaultFips: restricted
  ## @param global.clusterDomain Kubernetes cluster domain
  ##
  clusterDomain: cluster.local

## @section Common parameters

## @param nameOverride String to partially override nginx.fullname template (will maintain the release name)
##
nameOverride: ""
## @param fullnameOverride String to fully override nginx.fullname template
##
fullnameOverride: ""
## @param namespaceOverride String to fully override common.names.namespace
##
namespaceOverride: ""
## @param kubeVersion Force target Kubernetes version (using Helm capabilities if not set)
##
kubeVersion: ""
## @param clusterDomain Kubernetes Cluster Domain
##
clusterDomain: cluster.local
## @param extraDeploy Extra objects to deploy (value evaluated as a template)
##
extraDeploy: []
## @param commonLabels Add labels to all the deployed resources
##
commonLabels: {}
## @param commonAnnotations Add annotations to all the deployed resources
##
commonAnnotations: {}
## Enable diagnostic mode in the deployment(s)/statefulset(s)
##
diagnosticMode:
  ## @param diagnosticMode.enabled Enable diagnostic mode (all probes will be disabled and the command will be overridden)
  ##
  enabled: false
  ## @param diagnosticMode.command Command to override all containers in the the deployment(s)/statefulset(s)
  ##
  command:
    - sleep
  ## @param diagnosticMode.args Args to override all containers in the the deployment(s)/statefulset(s)
  ##
  args:
    - infinity
## @section NGINX parameters

## Bitnami NGINX image version
## ref: https://hub.docker.com/r/bitnami/nginx/tags/
## @param image.registry [default: REGISTRY_NAME] NGINX image registry
## @param image.repository [default: REPOSITORY_NAME/nginx] NGINX image repository
## @skip image.tag NGINX image tag (immutable tags are recommended)
## @param image.digest NGINX image digest in the way sha256:aa.... Please note this parameter, if set, will override the tag
## @param image.pullPolicy NGINX image pull policy
## @param image.pullSecrets Specify docker-registry secret names as an array
## @param image.debug Set to true if you would like to see extra information on logs
##
image:
  registry: registry-1.docker.io
  repository: bitnami/nginx
  tag: latest
  digest: ""
  ## Specify a imagePullPolicy
  ## ref: https://kubernetes.io/docs/concepts/containers/images/#pre-pulled-images
  ##
  pullPolicy: IfNotPresent
  ## Optionally specify an array of imagePullSecrets.
  ## Secrets must be manually created in the namespace.
  ## ref: https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/
  ## E.g.:
  ## pullSecrets:
  ##   - myRegistryKeySecretName
  ##
  pullSecrets: []
  ## Set to true if you would like to see extra information on logs
  ##
  debug: false
## @param enableDefaultInitContainers If set to false, disable all init containers except user-defined at `initContainer`.
##
enableDefaultInitContainers: true
## @param automountServiceAccountToken Mount Service Account token in pod
##
automountServiceAccountToken: false
## @param hostAliases Deployment pod host aliases
## https://kubernetes.io/docs/concepts/services-networking/add-entries-to-pod-etc-hosts-with-host-aliases/
##
hostAliases: []
## Command and args for running the container (set to default if not set). Use array form
## @param command Override default container command (useful when using custom images)
## @param args Override default container args (useful when using custom images)
##
command: []
args: []
## @param extraEnvVars Extra environment variables to be set on NGINX containers
## E.g:
## extraEnvVars:
##   - name: FOO
##     value: BAR
##
extraEnvVars: []
## @param extraEnvVarsCM ConfigMap with extra environment variables
##
extraEnvVarsCM: ""
## @param extraEnvVarsSecret Secret with extra environment variables
##
extraEnvVarsSecret: ""
## @section NGINX deployment parameters

## @param replicaCount Number of NGINX replicas to deploy
##
replicaCount: 1
## @param revisionHistoryLimit The number of old history to retain to allow rollback
##
revisionHistoryLimit: 10
## @param updateStrategy.type NGINX deployment strategy type
## @param updateStrategy.rollingUpdate NGINX deployment rolling update configuration parameters
## ref: https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#strategy
##
updateStrategy:
  type: RollingUpdate
  rollingUpdate: {}
## @param podLabels Additional labels for NGINX pods
## ref: https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/
##
podLabels: {}
## @param podAnnotations Annotations for NGINX pods
## ref: https://kubernetes.io/docs/concepts/overview/working-with-objects/annotations/
##
podAnnotations: {}
## @param podAffinityPreset Pod affinity preset. Ignored if `affinity` is set. Allowed values: `soft` or `hard`
## ref: https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#inter-pod-affinity-and-anti-affinity
##
podAffinityPreset: ""
## @param podAntiAffinityPreset Pod anti-affinity preset. Ignored if `affinity` is set. Allowed values: `soft` or `hard`
## Ref: https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#inter-pod-affinity-and-anti-affinity
##
podAntiAffinityPreset: soft
## Node affinity preset
## Ref: https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#node-affinity
##
nodeAffinityPreset:
  ## @param nodeAffinityPreset.type Node affinity preset type. Ignored if `affinity` is set. Allowed values: `soft` or `hard`
  ##
  type: ""
  ## @param nodeAffinityPreset.key Node label key to match Ignored if `affinity` is set.
  ## E.g.
  ## key: "kubernetes.io/e2e-az-name"
  ##
  key: ""
  ## @param nodeAffinityPreset.values Node label values to match. Ignored if `affinity` is set.
  ## E.g.
  ## values:
  ##   - e2e-az1
  ##   - e2e-az2
  ##
  values: []
## @param affinity Affinity for pod assignment
## ref: https://kubernetes.io/docs/concepts/configuration/assign-pod-node/#affinity-and-anti-affinity
## Note: podAffinityPreset, podAntiAffinityPreset, and  nodeAffinityPreset will be ignored when it's set
##
affinity: {}
## @param hostNetwork Specify if host network should be enabled for NGINX pod
##
hostNetwork: false
## @param hostIPC Specify if host IPC should be enabled for NGINX pod
##
hostIPC: false
## DNS-Pod services
## Ref: https://kubernetes.io/docs/concepts/services-networking/dns-pod-service/
## @param dnsPolicy Specifies the DNS policy for the NGINX pod
## DNS policies can be set on a per-Pod basis. Currently Kubernetes supports the following Pod-specific DNS policies.
## Available options: Default, ClusterFirst, ClusterFirstWithHostNet, None
## Ref: https://kubernetes.io/docs/concepts/services-networking/dns-pod-service/#pod-s-dns-policy
dnsPolicy: ""
## @param dnsConfig  Allows users more control on the DNS settings for a Pod. Required if `dnsPolicy` is set to `None`
## The dnsConfig field is optional and it can work with any dnsPolicy settings.
## Ref: https://kubernetes.io/docs/concepts/services-networking/dns-pod-service/#pod-dns-config
## E.g.
## dnsConfig:
##   nameservers:
##     - 192.0.2.1 # this is an example
##   searches:
##     - ns1.svc.cluster-domain.example
##     - my.dns.search.suffix
##   options:
##     - name: ndots
##       value: "2"
##     - name: edns0
dnsConfig: {}
## @param nodeSelector Node labels for pod assignment. Evaluated as a template.
## Ref: https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/
##
nodeSelector: {}
## @param tolerations Tolerations for pod assignment. Evaluated as a template.
## Ref: https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/
##
tolerations: []
## @param priorityClassName NGINX pods' priorityClassName
##
priorityClassName: ""
## @param schedulerName Name of the k8s scheduler (other than default)
## ref: https://kubernetes.io/docs/tasks/administer-cluster/configure-multiple-schedulers/
##
schedulerName: ""
## @param runtimeClassName Name of the runtime class to be used by pod(s)
## ref: https://kubernetes.io/docs/concepts/containers/runtime-class/
##
runtimeClassName: ""
## @param terminationGracePeriodSeconds In seconds, time the given to the NGINX pod needs to terminate gracefully
## ref: https://kubernetes.io/docs/concepts/workloads/pods/pod/#termination-of-pods
##
terminationGracePeriodSeconds: ""
## @param topologySpreadConstraints Topology Spread Constraints for pod assignment
## https://kubernetes.io/docs/concepts/workloads/pods/pod-topology-spread-constraints/
## The value is evaluated as a template
##
topologySpreadConstraints: []
## TLS settings
##
tls:
  ## @param tls.enabled Enable TLS transport
  ##
  enabled: true
  ## @param tls.autoGenerated Auto-generate self-signed certificates
  ##
  autoGenerated: true
  ## @param tls.existingSecret Name of a secret containing the certificates
  ##
  existingSecret: ""
  ## @param tls.certFilename Path of the certificate file when mounted as a secret
  ##
  certFilename: tls.crt
  ## @param tls.certKeyFilename Path of the certificate key file when mounted as a secret
  ##
  certKeyFilename: tls.key
  ## @param tls.certCAFilename Path of the certificate CA file when mounted as a secret
  ##
  certCAFilename: ca.crt
  ## @param tls.cert Content of the certificate to be added to the secret
  ##
  cert: ""
  ## @param tls.key Content of the certificate key to be added to the secret
  ##
  key: ""
  ## @param tls.ca Content of the certificate CA to be added to the secret
  ##
  ca: ""
## NGINX pods' Security Context.
## ref: https://kubernetes.io/docs/tasks/configure-pod-container/security-context/#set-the-security-context-for-a-pod
## @param podSecurityContext.enabled Enabled NGINX pods' Security Context
## @param podSecurityContext.fsGroupChangePolicy Set filesystem group change policy
## @param podSecurityContext.supplementalGroups Set filesystem extra groups
## @param podSecurityContext.fsGroup Set NGINX pod's Security Context fsGroup
## @param podSecurityContext.sysctls sysctl settings of the NGINX pods
##
podSecurityContext:
  enabled: true
  fsGroupChangePolicy: Always
  supplementalGroups: []
  fsGroup: 1001
  ## sysctl settings
  ## Example:
  ## sysctls:
  ## - name: net.core.somaxconn
  ##   value: "10000"
  ##
  sysctls: []
## NGINX containers' Security Context.
## ref: https://kubernetes.io/docs/tasks/configure-pod-container/security-context/#set-the-security-context-for-a-container
## @param containerSecurityContext.enabled Enabled containers' Security Context
## @param containerSecurityContext.seLinuxOptions [object,nullable] Set SELinux options in container
## @param containerSecurityContext.runAsUser Set containers' Security Context runAsUser
## @param containerSecurityContext.runAsGroup Set containers' Security Context runAsGroup
## @param containerSecurityContext.runAsNonRoot Set container's Security Context runAsNonRoot
## @param containerSecurityContext.privileged Set container's Security Context privileged
## @param containerSecurityContext.readOnlyRootFilesystem Set container's Security Context readOnlyRootFilesystem
## @param containerSecurityContext.allowPrivilegeEscalation Set container's Security Context allowPrivilegeEscalation
## @param containerSecurityContext.capabilities.drop List of capabilities to be dropped
## @param containerSecurityContext.seccompProfile.type Set container's Security Context seccomp profile
##
containerSecurityContext:
  enabled: true
  seLinuxOptions: {}
  runAsUser: 1001
  runAsGroup: 1001
  runAsNonRoot: true
  privileged: false
  readOnlyRootFilesystem: true
  allowPrivilegeEscalation: false
  capabilities:
    drop: ["ALL"]
  seccompProfile:
    type: "RuntimeDefault"
## Configures the ports NGINX listens on
## @param containerPorts.http Sets http port inside NGINX container
## @param containerPorts.https Sets https port inside NGINX container
##
containerPorts:
  http: 8080
  https: 8443
## @param extraContainerPorts Array of additional container ports for the NGINX container
## e.g:
## extraContainerPorts:
##   - name: grpc
##     containerPort: 4317
##
extraContainerPorts: []
## NGINX containers' resource requests and limits
## ref: https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/
## We usually recommend not to specify default resources and to leave this as a conscious
## choice for the user. This also increases chances charts run on environments with little
## resources, such as Minikube. If you do want to specify resources, uncomment the following
## lines, adjust them as necessary, and remove the curly braces after 'resources:'.
## @param resourcesPreset Set container resources according to one common preset (allowed values: none, nano, micro, small, medium, large, xlarge, 2xlarge). This is ignored if resources is set (resources is recommended for production).
## More information: https://github.com/bitnami/charts/blob/main/bitnami/common/templates/_resources.tpl#L15
##
resourcesPreset: "nano"
## @param resources Set container requests and limits for different resources like CPU or memory (essential for production workloads)
## Example:
## resources:
##   requests:
##     cpu: 2
##     memory: 512Mi
##   limits:
##     cpu: 3
##     memory: 1024Mi
##
resources: {}
## @param fips.openssl Configure OpenSSL FIPS mode: '', 'restricted', 'relaxed', 'off'. If empty (""), 'global.defaultFips' would be used
##
fips:
  openssl: ""
## NGINX containers' lifecycleHooks
## ref: https://kubernetes.io/docs/concepts/containers/container-lifecycle-hooks/
## ref: https://kubernetes.io/docs/tasks/configure-pod-container/attach-handler-lifecycle-event/
## If you do want to specify lifecycleHooks, uncomment the following
## lines, adjust them as necessary, and remove the curly braces on 'lifecycle:{}'.
## @param lifecycleHooks Optional lifecycleHooks for the NGINX container
lifecycleHooks: {}
## Example:
## postStart:
##   exec:
##     command: ["/bin/sh", "-c", "echo Hello from the postStart handler > /usr/share/message"]
## Example:
## preStop:
##   exec:
##     command: ["/bin/sleep", "20"]
##     command: ["/bin/sh","-c","nginx -s quit; while killall -0 nginx; do sleep 1; done"]

## NGINX containers' startup probe.
## ref: https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/#container-probes
## @param startupProbe.enabled Enable startupProbe
## @param startupProbe.initialDelaySeconds Initial delay seconds for startupProbe
## @param startupProbe.periodSeconds Period seconds for startupProbe
## @param startupProbe.timeoutSeconds Timeout seconds for startupProbe
## @param startupProbe.failureThreshold Failure threshold for startupProbe
## @param startupProbe.successThreshold Success threshold for startupProbe
##
startupProbe:
  enabled: false
  initialDelaySeconds: 30
  timeoutSeconds: 5
  periodSeconds: 10
  failureThreshold: 6
  successThreshold: 1
## NGINX containers' liveness probe.
## ref: https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/#container-probes
## @param livenessProbe.enabled Enable livenessProbe
## @param livenessProbe.initialDelaySeconds Initial delay seconds for livenessProbe
## @param livenessProbe.periodSeconds Period seconds for livenessProbe
## @param livenessProbe.timeoutSeconds Timeout seconds for livenessProbe
## @param livenessProbe.failureThreshold Failure threshold for livenessProbe
## @param livenessProbe.successThreshold Success threshold for livenessProbe
##
livenessProbe:
  enabled: true
  initialDelaySeconds: 30
  timeoutSeconds: 5
  periodSeconds: 10
  failureThreshold: 6
  successThreshold: 1
## NGINX containers' readiness probe.
## ref: https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/#container-probes
## @param readinessProbe.enabled Enable readinessProbe
## @param readinessProbe.path Request path for livenessProbe
## @param readinessProbe.initialDelaySeconds Initial delay seconds for readinessProbe
## @param readinessProbe.periodSeconds Period seconds for readinessProbe
## @param readinessProbe.timeoutSeconds Timeout seconds for readinessProbe
## @param readinessProbe.failureThreshold Failure threshold for readinessProbe
## @param readinessProbe.successThreshold Success threshold for readinessProbe
##
readinessProbe:
  enabled: true
  path: /
  initialDelaySeconds: 5
  timeoutSeconds: 3
  periodSeconds: 5
  failureThreshold: 3
  successThreshold: 1
## @param customStartupProbe Custom liveness probe for the Web component
##
customStartupProbe: {}
## @param customLivenessProbe Override default liveness probe
##
customLivenessProbe: {}
## @param customReadinessProbe Override default readiness probe
##
customReadinessProbe: {}
## Autoscaling parameters
## @param autoscaling.enabled Enable autoscaling for NGINX deployment
## @param autoscaling.minReplicas Minimum number of replicas to scale back
## @param autoscaling.maxReplicas Maximum number of replicas to scale out
## @param autoscaling.targetCPU Target CPU utilization percentage
## @param autoscaling.targetMemory Target Memory utilization percentage
##
autoscaling:
  enabled: false
  minReplicas: ""
  maxReplicas: ""
  targetCPU: ""
  targetMemory: ""
## @param extraVolumes Array to add extra volumes
##
extraVolumes: []
## @param extraVolumeMounts Array to add extra mount
##
extraVolumeMounts: []
## Pods Service Account
## ref: https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/
##
serviceAccount:
  ## @param serviceAccount.create Enable creation of ServiceAccount for nginx pod
  ##
  create: true
  ## @param serviceAccount.name The name of the ServiceAccount to use.
  ## If not set and create is true, a name is generated using the `common.names.fullname` template
  name: ""
  ## @param serviceAccount.annotations Annotations for service account. Evaluated as a template.
  ## Only used if `create` is `true`.
  ##
  annotations: {}
  ## @param serviceAccount.automountServiceAccountToken Auto-mount the service account token in the pod
  ##
  automountServiceAccountToken: false
## @param sidecars Sidecar parameters
## e.g:
## sidecars:
##   - name: your-image-name
##     image: your-image
##     imagePullPolicy: Always
##     ports:
##       - name: portname
##         containerPort: 1234
##
sidecars: []
## @param sidecarSingleProcessNamespace Enable sharing the process namespace with sidecars
## This will switch pod.spec.shareProcessNamespace parameter
##
sidecarSingleProcessNamespace: false
## @param initContainers Extra init containers
##
initContainers: []
## Pod Disruption Budget configuration
## ref: https://kubernetes.io/docs/tasks/run-application/configure-pdb/
##
pdb:
  ## @param pdb.create Created a PodDisruptionBudget
  ##
  create: true
  ## @param pdb.minAvailable Min number of pods that must still be available after the eviction.
  ## You can specify an integer or a percentage by setting the value to a string representation of a percentage (eg. "50%"). It will be disabled if set to 0
  ##
  minAvailable: ""
  ## @param pdb.maxUnavailable Max number of pods that can be unavailable after the eviction.
  ## You can specify an integer or a percentage by setting the value to a string representation of a percentage (eg. "50%"). It will be disabled if set to 0. Defaults to `1` if both `pdb.minAvailable` and `pdb.maxUnavailable` are empty.
  ##
  maxUnavailable: ""
## @section Custom NGINX application parameters

## Get the server static content from a git repository
## NOTE: This will override staticSiteConfigmap and staticSitePVC
##
cloneStaticSiteFromGit:
  ## @param cloneStaticSiteFromGit.enabled Get the server static content from a Git repository
  ##
  enabled: false
  ## Bitnami Git image version
  ## ref: https://hub.docker.com/r/bitnami/git/tags/
  ## @param cloneStaticSiteFromGit.image.registry [default: REGISTRY_NAME] Git image registry
  ## @param cloneStaticSiteFromGit.image.repository [default: REPOSITORY_NAME/git] Git image repository
  ## @skip cloneStaticSiteFromGit.image.tag Git image tag (immutable tags are recommended)
  ## @param cloneStaticSiteFromGit.image.digest Git image digest in the way sha256:aa.... Please note this parameter, if set, will override the tag
  ## @param cloneStaticSiteFromGit.image.pullPolicy Git image pull policy
  ## @param cloneStaticSiteFromGit.image.pullSecrets Specify docker-registry secret names as an array
  ##
  image:
    registry: registry-1.docker.io
    repository: bitnami/git
    tag: latest
    digest: ""
    ## Specify a imagePullPolicy
    ## ref: https://kubernetes.io/docs/concepts/containers/images/#pre-pulled-images
    ##
    pullPolicy: IfNotPresent
    ## Optionally specify an array of imagePullSecrets.
    ## Secrets must be manually created in the namespace.
    ## ref: https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/
    ## e.g:
    ## pullSecrets:
    ##   - myRegistryKeySecretName
    ##
    pullSecrets: []
  ## @param cloneStaticSiteFromGit.repository Git Repository to clone static content from
  ##
  repository: ""
  ## @param cloneStaticSiteFromGit.branch Git branch to checkout
  ##
  branch: ""
  ## @param cloneStaticSiteFromGit.interval Interval for sidecar container pull from the Git repository
  ##
  interval: 60
  ## FIPS configuration
  ## @param cloneStaticSiteFromGit.fips.openssl Configure FIPS mode for OpenSSL
  ##
  fips:
    openssl: ""
  ## Additional configuration for git-clone-repository initContainer
  ##
  gitClone:
    ## @param cloneStaticSiteFromGit.gitClone.command Override default container command for git-clone-repository
    ##
    command: []
    ## @param cloneStaticSiteFromGit.gitClone.args Override default container args for git-clone-repository
    ##
    args: []
  ## Additional configuration for the git-repo-syncer container
  ##
  gitSync:
    ## @param cloneStaticSiteFromGit.gitSync.command Override default container command for git-repo-syncer
    ##
    command: []
    ## @param cloneStaticSiteFromGit.gitSync.args Override default container args for git-repo-syncer
    ##
    args: []
    ## git-repo-syncer resource requests and limits
    ## ref: https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/
    ## @param cloneStaticSiteFromGit.gitSync.resourcesPreset Set container resources according to one common preset (allowed values: none, nano, micro, small, medium, large, xlarge, 2xlarge). This is ignored if cloneStaticSiteFromGit.gitSync.resources is set (cloneStaticSiteFromGit.gitSync.resources is recommended for production).
    ## More information: https://github.com/bitnami/charts/blob/main/bitnami/common/templates/_resources.tpl#L15
    ##
    resourcesPreset: "nano"
    ## @param cloneStaticSiteFromGit.gitSync.resources Set container requests and limits for different resources like CPU or memory (essential for production workloads)
    ## Example:
    ## resources:
    ##   requests:
    ##     cpu: 2
    ##     memory: 512Mi
    ##   limits:
    ##     cpu: 3
    ##     memory: 1024Mi
    ##
    resources: {}
  ## @param cloneStaticSiteFromGit.extraEnvVars Additional environment variables to set for the in the containers that clone static site from git
  ## E.g:
  ## extraEnvVars:
  ##   - name: FOO
  ##     value: BAR
  ##
  extraEnvVars: []
  ## @param cloneStaticSiteFromGit.extraEnvVarsSecret Secret with extra environment variables
  ##
  extraEnvVarsSecret: ""
  ## @param cloneStaticSiteFromGit.extraVolumeMounts Add extra volume mounts for the Git containers
  ## Useful to mount keys to connect through ssh. (normally used with extraVolumes)
  ## E.g:
  ## extraVolumeMounts:
  ##   - name: ssh-dir
  ##     mountPath: /root/.ssh/
  ##
  extraVolumeMounts: []
## @param serverBlock Custom server block to be added to NGINX configuration
## PHP-FPM example server block:
## serverBlock: |-
##   server {
##     listen 0.0.0.0:8080;
##     root /app;
##     location / {
##       index index.html index.php;
##     }
##     location ~ \.php$ {
##       fastcgi_pass phpfpm-server:9000;
##       fastcgi_index index.php;
##       include fastcgi.conf;
##     }
##   }
##
serverBlock: ""
## @param streamServerBlock Custom stream server block to be added to NGINX configuration
## streamServerBlock: |-
##   server {
##     listen 0.0.0.0:8080 udp;
##     proxy_pass localhost:9000;
##   }
##
streamServerBlock: ""
## @param existingServerBlockConfigmap ConfigMap with custom server block to be added to NGINX configuration
## NOTE: This will override serverBlock
##
existingServerBlockConfigmap: ""
## @param existingStreamServerBlockConfigmap ConfigMap with custom stream server block to be added to NGINX configuration
## NOTE: This will override streamServerBlock
##
existingStreamServerBlockConfigmap: ""
## Collection of NGINX context based includes
## e.g:
## contextIncludes:
##   main: |
##     load_module /opt/bitnami/nginx/modules/ngx_http_dav_module.so;
##
contextIncludes:
  ## @param contextIncludes.main Custom configuration for the main context
  ##
  main: ""
  ## @param contextIncludes.events Custom configuration for the events context
  ##
  events: ""
  ## @param contextIncludes.http Custom configuration for the http context
  ##
  http: ""
## @param existingContextMainConfigmaps List of existing ConfigMaps with custom main context configuration
## NOTE: These will be mounted alongside contextIncludes.main
## e.g:
## existingContextMainConfigmaps:
##   - "my-modules-config"
##   - "my-main-directives-config"
##
existingContextMainConfigmaps: []
## @param existingContextEventsConfigmaps List of existing ConfigMaps with custom events context configuration
## NOTE: These will be mounted alongside contextIncludes.events
##
existingContextEventsConfigmaps: []
## @param existingContextHttpConfigmaps List of existing ConfigMaps with custom http context configuration
## NOTE: These will be mounted alongside contextIncludes.http
##
existingContextHttpConfigmaps: []
## @param staticSiteConfigmap Name of existing ConfigMap with the server static site content
##
staticSiteConfigmap: ""
## @param staticSitePVC Name of existing PVC with the server static site content
## NOTE: This will override staticSiteConfigmap
##
staticSitePVC: ""
## @section Traffic Exposure parameters

## NGINX Service properties
##
service:
  ## @param service.type Service type
  ##
  type: LoadBalancer
  ## @param service.ports.http Service HTTP port
  ## @param service.ports.https Service HTTPS port
  ##
  ports:
    http: 80
    https: 443
  ##
  ## @param service.nodePorts [object] Specify the nodePort(s) value(s) for the LoadBalancer and NodePort service types.
  ## ref: https://kubernetes.io/docs/concepts/services-networking/service/#type-nodeport
  ##
  nodePorts:
    http: ""
    https: ""
  ## @param service.targetPort [object] Target port reference value for the Loadbalancer service types can be specified explicitly.
  ## Listeners for the Loadbalancer can be custom mapped to the http or https service.
  ## Example: Mapping the https listener to targetPort http [http: https]
  ##
  targetPort:
    http: http
    https: https
  ## @param service.clusterIP NGINX service Cluster IP
  ## e.g.:
  ## clusterIP: None
  ##
  clusterIP: ""
  ## @param service.loadBalancerIP LoadBalancer service IP address
  ## ref: https://kubernetes.io/docs/concepts/services-networking/service/#internal-load-balancer
  ##
  loadBalancerIP: ""
  ## @param service.loadBalancerSourceRanges NGINX service Load Balancer sources
  ## ref: https://kubernetes.io/docs/tasks/access-application-cluster/configure-cloud-provider-firewall/#restrict-access-for-loadbalancer-service
  ## e.g:
  ## loadBalancerSourceRanges:
  ##   - 10.10.10.0/24
  ##
  loadBalancerSourceRanges: []
  ## @param service.loadBalancerClass service Load Balancer class if service type is `LoadBalancer` (optional, cloud specific)
  ## ref: https://kubernetes.io/docs/concepts/services-networking/service/#type-loadbalancer
  ##
  loadBalancerClass: ""
  ## @param service.extraPorts Extra ports to expose (normally used with the `sidecar` value)
  ##
  extraPorts: []
  ## @param service.sessionAffinity Session Affinity for Kubernetes service, can be "None" or "ClientIP"
  ## If "ClientIP", consecutive client requests will be directed to the same Pod
  ## ref: https://kubernetes.io/docs/concepts/services-networking/service/#virtual-ips-and-service-proxies
  ##
  sessionAffinity: None
  ## @param service.sessionAffinityConfig Additional settings for the sessionAffinity. Ignored if `service.sessionAffinity` is `None`
  ## sessionAffinityConfig:
  ##   clientIP:
  ##     timeoutSeconds: 300
  ##
  sessionAffinityConfig: {}
  ## @param service.annotations Service annotations
  ## This can be used to set the LoadBalancer service type to internal only.
  ## ref: https://kubernetes.io/docs/concepts/services-networking/service/#internal-load-balancer
  ##
  annotations: {}
  ## @param service.externalTrafficPolicy Enable client source IP preservation
  ## ref https://kubernetes.io/docs/tasks/access-application-cluster/create-external-load-balancer/#preserving-the-client-source-ip
  ##
  externalTrafficPolicy: Cluster
## Network Policies
## Ref: https://kubernetes.io/docs/concepts/services-networking/network-policies/
##
networkPolicy:
  ## @param networkPolicy.enabled Specifies whether a NetworkPolicy should be created
  ##
  enabled: true
  ## @param networkPolicy.allowExternal Don't require server label for connections
  ## The Policy model to apply. When set to false, only pods with the correct
  ## server label will have network access to the ports server is listening
  ## on. When true, server will accept connections from any source
  ## (with the correct destination port).
  ##
  allowExternal: true
  ## @param networkPolicy.allowExternalEgress Allow the pod to access any range of port and all destinations.
  ##
  allowExternalEgress: true
  ## @param networkPolicy.extraIngress [array] Add extra ingress rules to the NetworkPolicy
  ## e.g:
  ## extraIngress:
  ##   - ports:
  ##       - port: 1234
  ##     from:
  ##       - podSelector:
  ##           - matchLabels:
  ##               - role: frontend
  ##       - podSelector:
  ##           - matchExpressions:
  ##               - key: role
  ##                 operator: In
  ##                 values:
  ##                   - frontend
  extraIngress: []
  ## @param networkPolicy.extraEgress [array] Add extra ingress rules to the NetworkPolicy (ignored if allowExternalEgress=true)
  ## e.g:
  ## extraEgress:
  ##   - ports:
  ##       - port: 1234
  ##     to:
  ##       - podSelector:
  ##           - matchLabels:
  ##               - role: frontend
  ##       - podSelector:
  ##           - matchExpressions:
  ##               - key: role
  ##                 operator: In
  ##                 values:
  ##                   - frontend
  ##
  extraEgress: []
  ## @param networkPolicy.ingressNSMatchLabels [object] Labels to match to allow traffic from other namespaces
  ## @param networkPolicy.ingressNSPodMatchLabels [object] Pod labels to match to allow traffic from other namespaces
  ##
  ingressNSMatchLabels: {}
  ingressNSPodMatchLabels: {}
## Gateway API HTTP routing parameters
## ref: https://gateway-api.sigs.k8s.io/guides/http-routing/
##
httpRoute:
  ## @param httpRoute.enabled Enable HTTPRoute generation for NGINX
  ##
  enabled: false
  ## @param httpRoute.annotations Additional annotations for the HTTPRoute resource
  ##
  annotations: {}
  ## @param httpRoute.labels Additional labels for the HTTPRoute resource
  ##
  labels: {}
  ## @param httpRoute.parentRefs Gateways the HTTPRoute is attached to. If unspecified, it'll be attached to Gateway named 'gateway' in the same namespace.
  ## e.g:
  ## parentRefs:
  ##   - name: my-gateway
  ##     sectionName: http
  ##     namespace: default
  ##
  parentRefs: []
  ## @param httpRoute.hostnames [array] List of hostnames matching HTTP header
  ##
  hostnames:
    - nginx.local
  ## @param httpRoute.matches [array] List of match rules applied to the HTTPRoute for the default svc backend reference
  ##
  matches:
    - path:
        type: PathPrefix
        value: /
  ## @param httpRoute.filters List of filter rules applied to the HTTPRoute for the default svc backend reference
  ##
  filters: []
  ## @param httpRoute.extraRules List of extra rules applied to the HTTPRoute
  ## e.g:
  ## extraRules:
  ##   - matches:
  ##       - path:
  ##           type: PathPrefix
  ##           value: /login
  ##     filters:
  ##       - type: RequestHeaderModifier
  ##         requestHeaderModifier:
  ##           set:
  ##             - name: My-Overwrite-Header
  ##               value: this-is-the-only-value
  ##           remove:
  ##             - User-Agent
  ##     backendRefs:
  ##       - name: nginx
  ##         port: 8080
  ##
  extraRules: []
## Gateway API BackendTLSPolicy parameters
## ref: https://gateway-api.sigs.k8s.io/guides/tls/#upstream-tls
##
backendTLSPolicy:
  ## @param backendTLSPolicy.enabled Enable BackendTLSPolicy generation for NGINX. Ignored if `tls.enabled` is `false`.
  ##
  enabled: false
  ## @param backendTLSPolicy.annotations Additional annotations for the BackendTLSPolicy resource
  ##
  annotations: {}
  ## @param backendTLSPolicy.labels Additional labels for the BackendTLSPolicy resource
  ##
  labels: {}
  ## @param backendTLSPolicy.hostname SNI to connect to the backend, it must match the certificate served by the backend unless SubjectAltNames are specified.
  ## By default, the FQDN of the backend service is used.
  ##
  hostname: ""
  ## @param backendTLSPolicy.subjectAltNames List of Subject Alternative Names (SANs) to verify the backend certificate against.
  ##
  subjectAltNames: []
## Configure the ingress resource that allows you to access the
## NGINX installation. Set up the URL
## ref: https://kubernetes.io/docs/concepts/services-networking/ingress/
##
ingress:
  ## @param ingress.enabled Set to true to enable ingress record generation
  ##
  enabled: false
  ## @param ingress.selfSigned Create a TLS secret for this ingress record using self-signed certificates generated by Helm
  ##
  selfSigned: false
  ## @param ingress.pathType Ingress path type
  ##
  pathType: ImplementationSpecific
  ## @param ingress.apiVersion Force Ingress API version (automatically detected if not set)
  ##
  apiVersion: ""
  ## @param ingress.hostname Default host for the ingress resource
  ##
  hostname: nginx.local
  ## @param ingress.path The Path to NGINX. You may need to set this to '/*' in order to use this with ALB ingress controllers.
  ##
  path: /
  ## @param ingress.annotations Additional annotations for the Ingress resource. To enable certificate autogeneration, place here your cert-manager annotations.
  ## For a full list of possible ingress annotations, please see
  ## ref: https://github.com/kubernetes/ingress-nginx/blob/main/docs/user-guide/nginx-configuration/annotations.md
  ## Use this parameter to set the required annotations for cert-manager, see
  ## ref: https://cert-manager.io/docs/usage/ingress/#supported-annotations
  ##
  ## e.g:
  ## annotations:
  ##   kubernetes.io/ingress.class: nginx
  ##   cert-manager.io/cluster-issuer: cluster-issuer-name
  ##
  annotations: {}
  ## @param ingress.ingressClassName Set the ingerssClassName on the ingress record for k8s 1.18+
  ## This is supported in Kubernetes 1.18+ and required if you have more than one IngressClass marked as the default for your cluster .
  ## ref: https://kubernetes.io/blog/2020/04/02/improvements-to-the-ingress-api-in-kubernetes-1.18/
  ##
  ingressClassName: ""
  ## @param ingress.tls Create TLS Secret
  ## TLS certificates will be retrieved from a TLS secret with name: {{- printf "%s-tls" .Values.ingress.hostname }}
  ## You can use the ingress.secrets parameter to create this TLS secret or relay on cert-manager to create it
  ##
  tls: false
  ## @param ingress.tlsWwwPrefix Adds www subdomain to default cert
  ## Creates tls host with ingress.hostname: {{ print "www.%s" .Values.ingress.hostname }}
  ## Is enabled if "nginx.ingress.kubernetes.io/from-to-www-redirect" is "true"
  tlsWwwPrefix: false
  ## @param ingress.extraHosts The list of additional hostnames to be covered with this ingress record.
  ## Most likely the hostname above will be enough, but in the event more hosts are needed, this is an array
  ## extraHosts:
  ## - name: nginx.local
  ##   path: /
  ##
  extraHosts: []
  ## @param ingress.extraPaths Any additional arbitrary paths that may need to be added to the ingress under the main host.
  ## For example: The ALB ingress controller requires a special rule for handling SSL redirection.
  ## extraPaths:
  ## - path: /*
  ##   backend:
  ##     serviceName: ssl-redirect
  ##     servicePort: use-annotation
  ##
  extraPaths: []
  ## @param ingress.extraTls The tls configuration for additional hostnames to be covered with this ingress record.
  ## see: https://kubernetes.io/docs/concepts/services-networking/ingress/#tls
  ## extraTls:
  ## - hosts:
  ##     - nginx.local
  ##   secretName: nginx.local-tls
  ##
  extraTls: []
  ## @param ingress.secrets If you're providing your own certificates, please use this to add the certificates as secrets
  ## key and certificate should start with -----BEGIN CERTIFICATE----- or
  ## -----BEGIN RSA PRIVATE KEY-----
  ##
  ## name should line up with a tlsSecret set further up
  ## If you're using cert-manager, this is unneeded, as it will create the secret for you if it is not set
  ##
  ## It is also possible to create and manage the certificates outside of this helm chart
  ## Please see README.md for more information
  ## e.g:
  ## - name: nginx.local-tls
  ##   key:
  ##   certificate:
  ##
  secrets: []
  ## @param ingress.extraRules The list of additional rules to be added to this ingress record. Evaluated as a template
  ## Useful when looking for additional customization, such as using different backend
  ##
  extraRules: []
## Health Ingress parameters
##
healthIngress:
  ## @param healthIngress.enabled Set to true to enable health ingress record generation
  ##
  enabled: false
  ## @param healthIngress.selfSigned Create a TLS secret for this ingress record using self-signed certificates generated by Helm
  ##
  selfSigned: false
  ## @param healthIngress.pathType Ingress path type
  ##
  pathType: ImplementationSpecific
  ## @param healthIngress.hostname When the health ingress is enabled, a host pointing to this will be created
  ##
  hostname: example.local
  ## @param healthIngress.path Default path for the ingress record
  ## NOTE: You may need to set this to '/*' in order to use this with ALB ingress controllers
  ##
  path: /
  ## @param healthIngress.annotations Additional annotations for the Ingress resource. To enable certificate autogeneration, place here your cert-manager annotations.
  ## For a full list of possible ingress annotations, please see
  ## ref: https://github.com/kubernetes/ingress-nginx/blob/main/docs/user-guide/nginx-configuration/annotations.md
  ## Use this parameter to set the required annotations for cert-manager, see
  ## ref: https://cert-manager.io/docs/usage/ingress/#supported-annotations
  ##
  ## e.g:
  ## annotations:
  ##   kubernetes.io/ingress.class: nginx
  ##   cert-manager.io/cluster-issuer: cluster-issuer-name
  ##
  annotations: {}
  ## @param healthIngress.tls Enable TLS configuration for the hostname defined at `healthIngress.hostname` parameter
  ## TLS certificates will be retrieved from a TLS secret with name: {{- printf "%s-tls" .Values.healthIngress.hostname }}
  ## You can use the healthIngress.secrets parameter to create this TLS secret, relay on cert-manager to create it, or
  ## let the chart create self-signed certificates for you
  ##
  tls: false
  ## @param healthIngress.extraHosts An array with additional hostname(s) to be covered with the ingress record
  ## e.g:
  ## extraHosts:
  ##   - name: example.local
  ##     path: /
  ##
  extraHosts: []
  ## @param healthIngress.extraPaths An array with additional arbitrary paths that may need to be added to the ingress under the main host
  ## e.g:
  ## extraPaths:
  ## - path: /*
  ##   backend:
  ##     serviceName: ssl-redirect
  ##     servicePort: use-annotation
  ##
  extraPaths: []
  ## @param healthIngress.extraTls TLS configuration for additional hostnames to be covered
  ## see: https://kubernetes.io/docs/concepts/services-networking/ingress/#tls
  ## E.g.
  ## extraTls:
  ##   - hosts:
  ##       - example.local
  ##     secretName: example.local-tls
  ##
  extraTls: []
  ## @param healthIngress.secrets TLS Secret configuration
  ## If you're providing your own certificates, please use this to add the certificates as secrets
  ## key and certificate should start with -----BEGIN CERTIFICATE----- or -----BEGIN RSA PRIVATE KEY-----
  ## name should line up with a secretName set further up
  ## If it is not set and you're using cert-manager, this is unneeded, as it will create the secret for you
  ## If it is not set and you're NOT using cert-manager either, self-signed certificates will be created
  ## It is also possible to create and manage the certificates outside of this helm chart
  ## Please see README.md for more information
  ##
  ## E.g.
  ## secrets:
  ##   - name: example.local-tls
  ##     key:
  ##     certificate:
  ##
  secrets: []
  ## @param healthIngress.ingressClassName IngressClass that will be be used to implement the Ingress (Kubernetes 1.18+)
  ## This is supported in Kubernetes 1.18+ and required if you have more than one IngressClass marked as the default for your cluster .
  ## ref: https://kubernetes.io/blog/2020/04/02/improvements-to-the-ingress-api-in-kubernetes-1.18/
  ##
  ingressClassName: ""
  ## @param healthIngress.extraRules The list of additional rules to be added to this ingress record. Evaluated as a template
  ## Useful when looking for additional customization, such as using different backend
  ##
  extraRules: []
## @section Metrics parameters

## Prometheus Exporter / Metrics
##
metrics:
  ## @param metrics.enabled Start a Prometheus exporter sidecar container
  ##
  enabled: false
  ## Bitnami NGINX Prometheus Exporter image
  ## ref: https://hub.docker.com/r/bitnami/nginx-exporter/tags/
  ## @param metrics.image.registry [default: REGISTRY_NAME] NGINX Prometheus exporter image registry
  ## @param metrics.image.repository [default: REPOSITORY_NAME/nginx-exporter] NGINX Prometheus exporter image repository
  ## @skip metrics.image.tag NGINX Prometheus exporter image tag (immutable tags are recommended)
  ## @param metrics.image.digest NGINX Prometheus exporter image digest in the way sha256:aa.... Please note this parameter, if set, will override the tag
  ## @param metrics.image.pullPolicy NGINX Prometheus exporter image pull policy
  ## @param metrics.image.pullSecrets Specify docker-registry secret names as an array
  ##
  image:
    registry: registry-1.docker.io
    repository: bitnami/nginx-exporter
    tag: latest
    digest: ""
    pullPolicy: IfNotPresent
    ## Optionally specify an array of imagePullSecrets.
    ## Secrets must be manually created in the namespace.
    ## ref: https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/
    ## e.g:
    ## pullSecrets:
    ##   - myRegistryKeySecretName
    ##
    pullSecrets: []
  ## @param metrics.port NGINX Container Status Port scraped by Prometheus Exporter
  ## Defaults to specified http port
  ##
  port: ""
  ## @param metrics.extraArgs Extra arguments for Prometheus exporter
  ## e.g:
  ## extraArgs:
  ##   - --nginx.timeout
  ##   - 5s
  ##
  extraArgs: []
  ## @param metrics.containerPorts.metrics Prometheus exporter container port
  ##
  containerPorts:
    metrics: 9113
  ## @param metrics.podAnnotations Additional annotations for NGINX Prometheus exporter pod(s)
  ## ref: https://kubernetes.io/docs/concepts/overview/working-with-objects/annotations/
  ##
  podAnnotations: {}
  ## Container Security Context
  ## ref: https://kubernetes.io/docs/tasks/configure-pod-container/security-context/
  ## @param metrics.securityContext.enabled Enabled NGINX Exporter containers' Security Context
  ## @param metrics.securityContext.seLinuxOptions [object,nullable] Set SELinux options in container
  ## @param metrics.securityContext.runAsUser Set NGINX Exporter container's Security Context runAsUser
  ## @param metrics.securityContext.runAsGroup Set NGINX Exporter container's Security Context runAsGroup
  ## @param metrics.securityContext.runAsNonRoot Set NGINX Exporter container's Security Context runAsNonRoot
  ## @param metrics.securityContext.privileged Set NGINX Exporter container's Security Context privileged
  ## @param metrics.securityContext.readOnlyRootFilesystem Set NGINX Exporter container's Security Context readOnlyRootFilesystem
  ## @param metrics.securityContext.allowPrivilegeEscalation Set NGINX Exporter container's Security Context allowPrivilegeEscalation
  ## @param metrics.securityContext.capabilities.drop List of capabilities to be dropped
  ## @param metrics.securityContext.seccompProfile.type Set NGINX Exporter container's Security Context seccomp profile
  ##
  securityContext:
    enabled: true
    seLinuxOptions: {}
    runAsUser: 1001
    runAsGroup: 1001
    runAsNonRoot: true
    privileged: false
    readOnlyRootFilesystem: true
    allowPrivilegeEscalation: false
    capabilities:
      drop: ["ALL"]
    seccompProfile:
      type: "RuntimeDefault"
  ## Prometheus exporter service parameters
  ##
  service:
    ## @param metrics.service.port NGINX Prometheus exporter service port
    ##
    port: 9113
    ## @param metrics.service.annotations [object] Annotations for the Prometheus exporter service
    ##
    annotations:
      prometheus.io/scrape: "true"
      prometheus.io/port: "{{ .Values.metrics.service.port }}"
  ## NGINX Prometheus exporter resource requests and limits
  ## ref: https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/
  ## We usually recommend not to specify default resources and to leave this as a conscious
  ## choice for the user. This also increases chances charts run on environments with little
  ## resources, such as Minikube. If you do want to specify resources, uncomment the following
  ## lines, adjust them as necessary, and remove the curly braces after 'resources:'.
  ## @param metrics.resourcesPreset Set container resources according to one common preset (allowed values: none, nano, micro, small, medium, large, xlarge, 2xlarge). This is ignored if metrics.resources is set (metrics.resources is recommended for production).
  ## More information: https://github.com/bitnami/charts/blob/main/bitnami/common/templates/_resources.tpl#L15
  ##
  resourcesPreset: "nano"
  ## @param metrics.resources Set container requests and limits for different resources like CPU or memory (essential for production workloads)
  ## Example:
  ## resources:
  ##   requests:
  ##     cpu: 2
  ##     memory: 512Mi
  ##   limits:
  ##     cpu: 3
  ##     memory: 1024Mi
  ##
  resources: {}
  ## @param metrics.fips.openssl Configure OpenSSL FIPS mode: '', 'restricted', 'relaxed', 'off'. If empty (""), 'global.defaultFips' would be used
  ## @param metrics.fips.golang Configure Golang FIPS mode: '', 'restricted', 'relaxed', 'off'. If empty (""), 'global.defaultFips' would be used
  ##
  fips:
    openssl: ""
    golang: restricted
  ## Prometheus Operator ServiceMonitor configuration
  ##
  serviceMonitor:
    ## @param metrics.serviceMonitor.enabled Creates a Prometheus Operator ServiceMonitor (also requires `metrics.enabled` to be `true`)
    ##
    enabled: false
    ## @param metrics.serviceMonitor.namespace Namespace in which Prometheus is running
    ##
    namespace: ""
    ## @param metrics.serviceMonitor.tlsConfig [object] TLS configuration used for scrape endpoints used by Prometheus
    ##
    tlsConfig: {}
    ## @param metrics.serviceMonitor.jobLabel The name of the label on the target service to use as the job name in prometheus.
    ##
    jobLabel: ""
    ## @param metrics.serviceMonitor.interval Interval at which metrics should be scraped.
    ## ref: https://github.com/coreos/prometheus-operator/blob/master/Documentation/api.md#endpoint
    ## e.g:
    ## interval: 10s
    ##
    interval: ""
    ## @param metrics.serviceMonitor.scrapeTimeout Timeout after which the scrape is ended
    ## ref: https://github.com/coreos/prometheus-operator/blob/master/Documentation/api.md#endpoint
    ## e.g:
    ## scrapeTimeout: 10s
    ##
    scrapeTimeout: ""
    ## @param metrics.serviceMonitor.selector Prometheus instance selector labels
    ## ref: https://techdocs.broadcom.com/us/en/vmware-tanzu/bitnami-secure-images/bitnami-secure-images/services/bsi-app-doc/apps-charts-kube-prometheus-index.html#-additional-scrape-configurations
    ##
    ## selector:
    ##   prometheus: my-prometheus
    ##
    selector: {}
    ## @param metrics.serviceMonitor.labels Additional labels that can be used so PodMonitor will be discovered by Prometheus
    ##
    labels: {}
    ## @param metrics.serviceMonitor.relabelings RelabelConfigs to apply to samples before scraping
    ##
    relabelings: []
    ## @param metrics.serviceMonitor.metricRelabelings MetricRelabelConfigs to apply to samples before ingestion
    ##
    metricRelabelings: []
    ## @param metrics.serviceMonitor.honorLabels honorLabels chooses the metric's labels on collisions with target labels
    ##
    honorLabels: false
  ## Prometheus Operator PrometheusRule configuration
  ##
  prometheusRule:
    ## @param metrics.prometheusRule.enabled if `true`, creates a Prometheus Operator PrometheusRule (also requires `metrics.enabled` to be `true` and `metrics.prometheusRule.rules`)
    ##
    enabled: false
    ## @param metrics.prometheusRule.namespace Namespace for the PrometheusRule Resource (defaults to the Release Namespace)
    ##
    namespace: ""
    ## @param metrics.prometheusRule.additionalLabels Additional labels that can be used so PrometheusRule will be discovered by Prometheus
    ##
    additionalLabels: {}
    ## @param metrics.prometheusRule.rules Prometheus Rule definitions
    ##   - alert: LowInstance
    ##     expr: up{service="{{ template "common.names.fullname" . }}"} < 1
    ##     for: 1m
    ##     labels:
    ##       severity: critical
    ##     annotations:
    ##       description: Service {{ template "common.names.fullname" . }} Tomcat is down since 1m.
    ##       summary: Tomcat instance is down.
    ##
    rules: []
  ## @param metrics.customLivenessProbe Override default metrics liveness probe
  ##
  customLivenessProbe: {}
  ## NGINX metrics containers' liveness probe.
  ## ref: https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/#container-probes
  ## @param metrics.livenessProbe.enabled Enable livenessProbe
  ## @param metrics.livenessProbe.initialDelaySeconds Initial delay seconds for livenessProbe
  ## @param metrics.livenessProbe.timeoutSeconds Timeout seconds for livenessProbe
  ## @param metrics.livenessProbe.periodSeconds Period seconds for livenessProbe
  ## @param metrics.livenessProbe.failureThreshold Failure threshold for livenessProbe
  ## @param metrics.livenessProbe.successThreshold Success threshold for livenessProbe
  ##
  livenessProbe:
    enabled: true
    initialDelaySeconds: 30
    timeoutSeconds: 5
    periodSeconds: 10
    failureThreshold: 2
    successThreshold: 1
  ## @param metrics.customReadinessProbe Override default metrics readiness probe
  ##
  customReadinessProbe: {}
  ## NGINX metrics containers' readiness probe.
  ## ref: https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/#container-probes
  ## @param metrics.readinessProbe.enabled Enable readinessProbe
  ## @param metrics.readinessProbe.initialDelaySeconds Initial delay seconds for readinessProbe
  ## @param metrics.readinessProbe.timeoutSeconds Timeout seconds for readinessProbe
  ## @param metrics.readinessProbe.periodSeconds Period seconds for readinessProbe
  ## @param metrics.readinessProbe.failureThreshold Failure threshold for readinessProbe
  ## @param metrics.readinessProbe.successThreshold Success threshold for readinessProbe
  ##
  readinessProbe:
    enabled: true
    initialDelaySeconds: 5
    timeoutSeconds: 3
    periodSeconds: 30
    failureThreshold: 2
    successThreshold: 1
  ## @param metrics.customStartupProbe Override default metrics startup probe
  ##
  customStartupProbe: {}
  ## NGINX metrics containers' startup probe.
  ## ref: https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/#container-probes
  ## @param metrics.startupProbe.enabled Enable startupProbe
  ## @param metrics.startupProbe.initialDelaySeconds Initial delay seconds for startupProbe
  ## @param metrics.startupProbe.timeoutSeconds Timeout seconds for startupProbe
  ## @param metrics.startupProbe.periodSeconds Period seconds for startupProbe
  ## @param metrics.startupProbe.failureThreshold Failure threshold for startupProbe
  ## @param metrics.startupProbe.successThreshold Success threshold for startupProbe
  ##
  startupProbe:
    enabled: false
    initialDelaySeconds: 5
    timeoutSeconds: 3
    periodSeconds: 5
    failureThreshold: 10
    successThreshold: 1

ubuntu@ip-172-31-6-80:~$ helm install my-nginx-custom bitnami/nginx \
  --set replicaCount=3 \
  --set service.type=NodePort
NAME: my-nginx-custom
LAST DEPLOYED: Tue Sep 22 10:14:49 2026
NAMESPACE: default
STATUS: deployed
REVISION: 1
DESCRIPTION: Install complete
TEST SUITE: None
NOTES:
CHART NAME: nginx
CHART VERSION: 25.1.14
APP VERSION: 1.31.6

⚠ WARNING: Since August 28th, 2025, only a limited subset of images/charts are available for free.
    Subscribe to Bitnami Secure Images to receive continued support and security updates.
    More info at https://bitnami.com and https://github.com/bitnami/containers/issues/83267

** Please be patient while the chart is being deployed **
NGINX can be accessed through the following DNS name from within your cluster:

    my-nginx-custom.default.svc.cluster.local (port 80)

To access NGINX from outside the cluster, follow the steps below:

1. Get the NGINX URL by running these commands:

    export NODE_PORT=$(kubectl get --namespace default -o jsonpath="{.spec.ports[0].nodePort}" services my-nginx-custom)
    export NODE_IP=$(kubectl get nodes --namespace default -o jsonpath="{.items[0].status.addresses[0].address}")
    echo "http://${NODE_IP}:${NODE_PORT}"
WARNING: Rolling tag detected (bitnami/nginx:latest), please note that it is strongly recommended to avoid using rolling tags in a production environment.
+info https://techdocs.broadcom.com/us/en/vmware-tanzu/bitnami-secure-images/bitnami-secure-images/services/bsi-doc/apps-tutorials-understand-rolling-tags-containers-index.html
WARNING: Rolling tag detected (bitnami/git:latest), please note that it is strongly recommended to avoid using rolling tags in a production environment.
+info https://techdocs.broadcom.com/us/en/vmware-tanzu/bitnami-secure-images/bitnami-secure-images/services/bsi-doc/apps-tutorials-understand-rolling-tags-containers-index.html
WARNING: Rolling tag detected (bitnami/nginx-exporter:latest), please note that it is strongly recommended to avoid using rolling tags in a production environment.
+info https://techdocs.broadcom.com/us/en/vmware-tanzu/bitnami-secure-images/bitnami-secure-images/services/bsi-doc/apps-tutorials-understand-rolling-tags-containers-index.html

WARNING: There are "resources" sections in the chart not set. Using "resourcesPreset" is not recommended for production. For production installations, please set the following values according to your workload needs:
  - cloneStaticSiteFromGit.gitSync.resources
  - resources
+info https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/
ubuntu@ip-172-31-6-80:~$ kubectl get all
NAME                                   READY   STATUS    RESTARTS   AGE
pod/my-nginx-54c75f846f-lbrvp          1/1     Running   0          19m
pod/my-nginx-custom-6f65c7ff75-97ttz   1/1     Running   0          12s
pod/my-nginx-custom-6f65c7ff75-nh7qh   1/1     Running   0          12s
pod/my-nginx-custom-6f65c7ff75-vpwtm   1/1     Running   0          12s

NAME                      TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)                      AGE
service/kubernetes        ClusterIP      10.96.0.1       <none>        443/TCP                      3d15h
service/my-nginx          LoadBalancer   10.96.230.217   <pending>     80:30277/TCP,443:32060/TCP   19m
service/my-nginx-custom   NodePort       10.96.46.4      <none>        80:30118/TCP,443:32420/TCP   13s

NAME                              READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/my-nginx          1/1     1            1           19m
deployment.apps/my-nginx-custom   3/3     3            3           13s

NAME                                         DESIRED   CURRENT   READY   AGE
replicaset.apps/my-nginx-54c75f846f          1         1         1       19m
replicaset.apps/my-nginx-custom-6f65c7ff75   3         3         3       13s
ubuntu@ip-172-31-6-80:~$ helm list
NAME            NAMESPACE       REVISION        UPDATED                                 STATUS          CHART           APP VERSION
my-nginx        default         1               2026-09-22 09:55:16.183588943 +0000 UTC deployed        nginx-25.1.14   1.31.6
my-nginx-custom default         1               2026-09-22 10:14:49.925614459 +0000 UTC deployed        nginx-25.1.14   1.31.6


```

### Create a `custom-values.yaml` file with `replicaCount`, Service type, and resource limits, install another release using `-f custom-values.yaml`, and verify the configured overrides with `helm get values <release-name>`.

```bash
ubuntu@ip-172-31-6-80:~$ helm list
NAME            NAMESPACE       REVISION        UPDATED                                 STATUS          CHART           APP VERSION
my-nginx        default         1               2026-09-22 09:55:16.183588943 +0000 UTC deployed        nginx-25.1.14   1.31.6
ubuntu@ip-172-31-6-80:~$ helm install my-nginx-custom bitnami/nginx -f custom-values.yaml
NAME: my-nginx-custom
LAST DEPLOYED: Tue Sep 22 10:26:27 2026
NAMESPACE: default
STATUS: deployed
REVISION: 1
DESCRIPTION: Install complete
TEST SUITE: None
NOTES:
CHART NAME: nginx
CHART VERSION: 25.1.14
APP VERSION: 1.31.6

⚠ WARNING: Since August 28th, 2025, only a limited subset of images/charts are available for free.
    Subscribe to Bitnami Secure Images to receive continued support and security updates.
    More info at https://bitnami.com and https://github.com/bitnami/containers/issues/83267

** Please be patient while the chart is being deployed **
NGINX can be accessed through the following DNS name from within your cluster:

    my-nginx-custom.default.svc.cluster.local (port 80)

To access NGINX from outside the cluster, follow the steps below:

1. Get the NGINX URL by running these commands:

    export NODE_PORT=$(kubectl get --namespace default -o jsonpath="{.spec.ports[0].nodePort}" services my-nginx-custom)
    export NODE_IP=$(kubectl get nodes --namespace default -o jsonpath="{.items[0].status.addresses[0].address}")
    echo "http://${NODE_IP}:${NODE_PORT}"
WARNING: Rolling tag detected (bitnami/nginx:latest), please note that it is strongly recommended to avoid using rolling tags in a production environment.
+info https://techdocs.broadcom.com/us/en/vmware-tanzu/bitnami-secure-images/bitnami-secure-images/services/bsi-doc/apps-tutorials-understand-rolling-tags-containers-index.html
WARNING: Rolling tag detected (bitnami/git:latest), please note that it is strongly recommended to avoid using rolling tags in a production environment.
+info https://techdocs.broadcom.com/us/en/vmware-tanzu/bitnami-secure-images/bitnami-secure-images/services/bsi-doc/apps-tutorials-understand-rolling-tags-containers-index.html
WARNING: Rolling tag detected (bitnami/nginx-exporter:latest), please note that it is strongly recommended to avoid using rolling tags in a production environment.
+info https://techdocs.broadcom.com/us/en/vmware-tanzu/bitnami-secure-images/bitnami-secure-images/services/bsi-doc/apps-tutorials-understand-rolling-tags-containers-index.html

WARNING: There are "resources" sections in the chart not set. Using "resourcesPreset" is not recommended for production. For production installations, please set the following values according to your workload needs:
  - cloneStaticSiteFromGit.gitSync.resources
+info https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/
ubuntu@ip-172-31-6-80:~$ kubectl get all
NAME                                   READY   STATUS    RESTARTS   AGE
pod/my-nginx-54c75f846f-lbrvp          1/1     Running   0          31m
pod/my-nginx-custom-7c5c88567f-7mwg5   0/1     Pending   0          18s
pod/my-nginx-custom-7c5c88567f-d2cxx   0/1     Pending   0          18s
pod/my-nginx-custom-7c5c88567f-qgtmx   1/1     Running   0          18s

NAME                      TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)                      AGE
service/kubernetes        ClusterIP      10.96.0.1       <none>        443/TCP                      3d15h
service/my-nginx          LoadBalancer   10.96.230.217   <pending>     80:30277/TCP,443:32060/TCP   31m
service/my-nginx-custom   NodePort       10.96.194.64    <none>        80:30216/TCP,443:31670/TCP   18s

NAME                              READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/my-nginx          1/1     1            1           31m
deployment.apps/my-nginx-custom   1/3     3            1           18s

NAME                                         DESIRED   CURRENT   READY   AGE
replicaset.apps/my-nginx-54c75f846f          1         1         1       31m
replicaset.apps/my-nginx-custom-7c5c88567f   3         3         1       18s
ubuntu@ip-172-31-6-80:~$
ubuntu@ip-172-31-6-80:~$ kubectl get all
NAME                                   READY   STATUS    RESTARTS   AGE
pod/my-nginx-54c75f846f-lbrvp          1/1     Running   0          31m
pod/my-nginx-custom-7c5c88567f-7mwg5   0/1     Pending   0          26s
pod/my-nginx-custom-7c5c88567f-d2cxx   0/1     Pending   0          26s
pod/my-nginx-custom-7c5c88567f-qgtmx   1/1     Running   0          26s

NAME                      TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)                      AGE
service/kubernetes        ClusterIP      10.96.0.1       <none>        443/TCP                      3d15h
service/my-nginx          LoadBalancer   10.96.230.217   <pending>     80:30277/TCP,443:32060/TCP   31m
service/my-nginx-custom   NodePort       10.96.194.64    <none>        80:30216/TCP,443:31670/TCP   26s

NAME                              READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/my-nginx          1/1     1            1           31m
deployment.apps/my-nginx-custom   1/3     3            1           26s

NAME                                         DESIRED   CURRENT   READY   AGE
replicaset.apps/my-nginx-54c75f846f          1         1         1       31m
replicaset.apps/my-nginx-custom-7c5c88567f   3         3         1       26s
ubuntu@ip-172-31-6-80:~$ kubectl get all
NAME                                   READY   STATUS    RESTARTS   AGE
pod/my-nginx-54c75f846f-lbrvp          1/1     Running   0          31m
pod/my-nginx-custom-7c5c88567f-7mwg5   0/1     Pending   0          37s
pod/my-nginx-custom-7c5c88567f-d2cxx   0/1     Pending   0          37s
pod/my-nginx-custom-7c5c88567f-qgtmx   1/1     Running   0          37s

NAME                      TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)                      AGE
service/kubernetes        ClusterIP      10.96.0.1       <none>        443/TCP                      3d15h
service/my-nginx          LoadBalancer   10.96.230.217   <pending>     80:30277/TCP,443:32060/TCP   31m
service/my-nginx-custom   NodePort       10.96.194.64    <none>        80:30216/TCP,443:31670/TCP   37s

NAME                              READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/my-nginx          1/1     1            1           31m
deployment.apps/my-nginx-custom   1/3     3            1           37s

NAME                                         DESIRED   CURRENT   READY   AGE
replicaset.apps/my-nginx-54c75f846f          1         1         1       31m
replicaset.apps/my-nginx-custom-7c5c88567f   3         3         1       37s
ubuntu@ip-172-31-6-80:~$
ubuntu@ip-172-31-6-80:~$
ubuntu@ip-172-31-6-80:~$
ubuntu@ip-172-31-6-80:~$ kubectl get all
NAME                                   READY   STATUS    RESTARTS   AGE
pod/my-nginx-54c75f846f-lbrvp          1/1     Running   0          31m
pod/my-nginx-custom-7c5c88567f-7mwg5   0/1     Pending   0          42s
pod/my-nginx-custom-7c5c88567f-d2cxx   0/1     Pending   0          42s
pod/my-nginx-custom-7c5c88567f-qgtmx   1/1     Running   0          42s

NAME                      TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)                      AGE
service/kubernetes        ClusterIP      10.96.0.1       <none>        443/TCP                      3d15h
service/my-nginx          LoadBalancer   10.96.230.217   <pending>     80:30277/TCP,443:32060/TCP   31m
service/my-nginx-custom   NodePort       10.96.194.64    <none>        80:30216/TCP,443:31670/TCP   42s

NAME                              READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/my-nginx          1/1     1            1           31m
deployment.apps/my-nginx-custom   1/3     3            1           42s

NAME                                         DESIRED   CURRENT   READY   AGE
replicaset.apps/my-nginx-54c75f846f          1         1         1       31m
replicaset.apps/my-nginx-custom-7c5c88567f   3         3         1       42s
ubuntu@ip-172-31-6-80:~$
ubuntu@ip-172-31-6-80:~$
ubuntu@ip-172-31-6-80:~$ kubectl logs pod/my-nginx-custom-7c5c88567f-7mwg5
Defaulted container "nginx" out of: nginx, preserve-logs-symlinks (init)
ubuntu@ip-172-31-6-80:~$
ubuntu@ip-172-31-6-80:~$
ubuntu@ip-172-31-6-80:~$ kubectl get all
NAME                                   READY   STATUS    RESTARTS   AGE
pod/my-nginx-54c75f846f-lbrvp          1/1     Running   0          32m
pod/my-nginx-custom-7c5c88567f-7mwg5   0/1     Pending   0          71s
pod/my-nginx-custom-7c5c88567f-d2cxx   0/1     Pending   0          71s
pod/my-nginx-custom-7c5c88567f-qgtmx   1/1     Running   0          71s

NAME                      TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)                      AGE
service/kubernetes        ClusterIP      10.96.0.1       <none>        443/TCP                      3d15h
service/my-nginx          LoadBalancer   10.96.230.217   <pending>     80:30277/TCP,443:32060/TCP   32m
service/my-nginx-custom   NodePort       10.96.194.64    <none>        80:30216/TCP,443:31670/TCP   71s

NAME                              READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/my-nginx          1/1     1            1           32m
deployment.apps/my-nginx-custom   1/3     3            1           71s

NAME                                         DESIRED   CURRENT   READY   AGE
replicaset.apps/my-nginx-54c75f846f          1         1         1       32m
replicaset.apps/my-nginx-custom-7c5c88567f   3         3         1       71s
ubuntu@ip-172-31-6-80:~$
ubuntu@ip-172-31-6-80:~$
ubuntu@ip-172-31-6-80:~$
ubuntu@ip-172-31-6-80:~$ kubectl get all
NAME                                   READY   STATUS    RESTARTS   AGE
pod/my-nginx-54c75f846f-lbrvp          1/1     Running   0          33m
pod/my-nginx-custom-7c5c88567f-7mwg5   0/1     Pending   0          2m22s
pod/my-nginx-custom-7c5c88567f-d2cxx   0/1     Pending   0          2m22s
pod/my-nginx-custom-7c5c88567f-qgtmx   1/1     Running   0          2m22s

NAME                      TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)                      AGE
service/kubernetes        ClusterIP      10.96.0.1       <none>        443/TCP                      3d15h
service/my-nginx          LoadBalancer   10.96.230.217   <pending>     80:30277/TCP,443:32060/TCP   33m
service/my-nginx-custom   NodePort       10.96.194.64    <none>        80:30216/TCP,443:31670/TCP   2m22s

NAME                              READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/my-nginx          1/1     1            1           33m
deployment.apps/my-nginx-custom   1/3     3            1           2m22s

NAME                                         DESIRED   CURRENT   READY   AGE
replicaset.apps/my-nginx-54c75f846f          1         1         1       33m
replicaset.apps/my-nginx-custom-7c5c88567f   3         3         1       2m22s
ubuntu@ip-172-31-6-80:~$
ubuntu@ip-172-31-6-80:~$
ubuntu@ip-172-31-6-80:~$
ubuntu@ip-172-31-6-80:~$
ubuntu@ip-172-31-6-80:~$ kubectl describe pod my-nginx-custom-7c5c88567f-7mwg5
Name:             my-nginx-custom-7c5c88567f-7mwg5
Namespace:        default
Priority:         0
Service Account:  my-nginx-custom
Node:             <none>
Labels:           app.kubernetes.io/instance=my-nginx-custom
                  app.kubernetes.io/managed-by=Helm
                  app.kubernetes.io/name=nginx
                  app.kubernetes.io/version=1.31.6
                  helm.sh/chart=nginx-25.1.14
                  pod-template-hash=7c5c88567f
Annotations:      <none>
Status:           Pending
IP:
IPs:              <none>
Controlled By:    ReplicaSet/my-nginx-custom-7c5c88567f
Init Containers:
  preserve-logs-symlinks:
    Image:           registry-1.docker.io/bitnami/nginx:latest
    Port:            <none>
    Host Port:       <none>
    SeccompProfile:  RuntimeDefault
    Command:
      /bin/bash
    Args:
      -ec
      #!/bin/bash
      . /opt/bitnami/scripts/libfs.sh
      # We copy the logs folder because it has symlinks to stdout and stderr
      if ! is_dir_empty /opt/bitnami/nginx/logs; then
        cp -r /opt/bitnami/nginx/logs /emptydir/app-logs-dir
      fi

    Limits:
      cpu:     500m
      memory:  512Mi
    Requests:
      cpu:     500m
      memory:  512Mi
    Environment:
      OPENSSL_FIPS:  yes
    Mounts:
      /emptydir from empty-dir (rw)
Containers:
  nginx:
    Image:           registry-1.docker.io/bitnami/nginx:latest
    Ports:           8080/TCP (http), 8443/TCP (https)
    Host Ports:      0/TCP (http), 0/TCP (https)
    SeccompProfile:  RuntimeDefault
    Limits:
      cpu:     500m
      memory:  512Mi
    Requests:
      cpu:      500m
      memory:   512Mi
    Liveness:   tcp-socket :http delay=30s timeout=5s period=10s successThreshold=1 failureThreshold=6
    Readiness:  http-get http://:http/ delay=5s timeout=3s period=5s successThreshold=1 failureThreshold=3
    Environment:
      BITNAMI_DEBUG:            false
      NGINX_HTTP_PORT_NUMBER:   8080
      OPENSSL_FIPS:             yes
      NGINX_HTTPS_PORT_NUMBER:  8443
    Mounts:
      /certs from certificate (rw)
      /opt/bitnami/nginx/conf from empty-dir (rw,path="app-conf-dir")
      /opt/bitnami/nginx/logs from empty-dir (rw,path="app-logs-dir")
      /opt/bitnami/nginx/tmp from empty-dir (rw,path="app-tmp-dir")
      /tmp from empty-dir (rw,path="tmp-dir")
Conditions:
  Type           Status
  PodScheduled   False
Volumes:
  empty-dir:
    Type:       EmptyDir (a temporary directory that shares a pod's lifetime)
    Medium:
    SizeLimit:  <unset>
  certificate:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  my-nginx-custom-tls
    Optional:    false
QoS Class:       Guaranteed
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                 node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type     Reason            Age    From               Message
  ----     ------            ----   ----               -------
  Warning  FailedScheduling  2m27s  default-scheduler  0/1 nodes are available: 1 Insufficient cpu. preemption: 0/1 nodes are available: 1 No preemption victims found for incoming pod.
ubuntu@ip-172-31-6-80:~$ helm get values my-nginx-custom
USER-SUPPLIED VALUES:
replicaCount: 3
resources:
  limits:
    cpu: 500m
    memory: 512Mi
service:
  type: NodePort
ubuntu@ip-172-31-6-80:~$

```
### Verify: Does the values file release have the correct replicas and service type?
 **the values were applied correctly**. The only issue is that **2 of the 3 pods are currently Pending**, so the cluster doesn't currently have enough available resources to run all three replicas.

 ## Task 5: Upgrade and Rollback

 ### Upgrade the release with helm upgrade my-nginx bitnami/nginx --set replicaCount=5, check the release history using helm history my-nginx, roll back to revision 1 with helm rollback my-nginx 1, then check the history again to confirm that the rollback creates a new revision (3) rather than overwriting revision 2.

 ```bash
ubuntu@ip-172-31-6-80:~$ helm upgrade my-nginx bitnami/nginx --set replicaCount=5
Release "my-nginx" has been upgraded. Happy Helming!
NAME: my-nginx
LAST DEPLOYED: Tue Sep 22 10:36:15 2026
NAMESPACE: default
STATUS: deployed
REVISION: 2
DESCRIPTION: Upgrade complete
TEST SUITE: None
NOTES:
CHART NAME: nginx
CHART VERSION: 25.1.14
APP VERSION: 1.31.6

⚠ WARNING: Since August 28th, 2025, only a limited subset of images/charts are available for free.
    Subscribe to Bitnami Secure Images to receive continued support and security updates.
    More info at https://bitnami.com and https://github.com/bitnami/containers/issues/83267

** Please be patient while the chart is being deployed **
NGINX can be accessed through the following DNS name from within your cluster:

    my-nginx.default.svc.cluster.local (port 80)

To access NGINX from outside the cluster, follow the steps below:

1. Get the NGINX URL by running these commands:

  NOTE: It may take a few minutes for the LoadBalancer IP to be available.
        Watch the status with: 'kubectl get svc --namespace default -w my-nginx'

    export SERVICE_PORT=$(kubectl get --namespace default -o jsonpath="{.spec.ports[0].port}" services my-nginx)
    export SERVICE_IP=$(kubectl get svc --namespace default my-nginx -o jsonpath='{.status.loadBalancer.ingress[0].ip}')
    echo "http://${SERVICE_IP}:${SERVICE_PORT}"
WARNING: Rolling tag detected (bitnami/nginx:latest), please note that it is strongly recommended to avoid using rolling tags in a production environment.
+info https://techdocs.broadcom.com/us/en/vmware-tanzu/bitnami-secure-images/bitnami-secure-images/services/bsi-doc/apps-tutorials-understand-rolling-tags-containers-index.html
WARNING: Rolling tag detected (bitnami/git:latest), please note that it is strongly recommended to avoid using rolling tags in a production environment.
+info https://techdocs.broadcom.com/us/en/vmware-tanzu/bitnami-secure-images/bitnami-secure-images/services/bsi-doc/apps-tutorials-understand-rolling-tags-containers-index.html
WARNING: Rolling tag detected (bitnami/nginx-exporter:latest), please note that it is strongly recommended to avoid using rolling tags in a production environment.
+info https://techdocs.broadcom.com/us/en/vmware-tanzu/bitnami-secure-images/bitnami-secure-images/services/bsi-doc/apps-tutorials-understand-rolling-tags-containers-index.html

WARNING: There are "resources" sections in the chart not set. Using "resourcesPreset" is not recommended for production. For production installations, please set the following values according to your workload needs:
  - cloneStaticSiteFromGit.gitSync.resources
  - resources
+info https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/
ubuntu@ip-172-31-6-80:~$ kubectl get all
NAME                                   READY   STATUS    RESTARTS   AGE
pod/my-nginx-54c75f846f-4z5r9          0/1     Running   0          5s
pod/my-nginx-54c75f846f-jfq44          0/1     Running   0          5s
pod/my-nginx-54c75f846f-lbrvp          1/1     Running   0          41m
pod/my-nginx-54c75f846f-p9zdq          0/1     Running   0          5s
pod/my-nginx-54c75f846f-xqwn6          0/1     Pending   0          4s
pod/my-nginx-custom-7c5c88567f-7mwg5   0/1     Pending   0          9m53s
pod/my-nginx-custom-7c5c88567f-d2cxx   0/1     Pending   0          9m53s
pod/my-nginx-custom-7c5c88567f-qgtmx   1/1     Running   0          9m53s

NAME                      TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)                      AGE
service/kubernetes        ClusterIP      10.96.0.1       <none>        443/TCP                      3d15h
service/my-nginx          LoadBalancer   10.96.230.217   <pending>     80:30277/TCP,443:32060/TCP   41m
service/my-nginx-custom   NodePort       10.96.194.64    <none>        80:30216/TCP,443:31670/TCP   9m53s

NAME                              READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/my-nginx          1/5     5            1           41m
deployment.apps/my-nginx-custom   1/3     3            1           9m53s

NAME                                         DESIRED   CURRENT   READY   AGE
replicaset.apps/my-nginx-54c75f846f          5         5         1       41m
replicaset.apps/my-nginx-custom-7c5c88567f   3         3         1       9m53s
ubuntu@ip-172-31-6-80:~$ helm history my-nginx
REVISION        UPDATED                         STATUS          CHART           APP VERSION     DESCRIPTION
1               Tue Sep 22 09:55:16 2026        superseded      nginx-25.1.14   1.31.6          Install complete
2               Tue Sep 22 10:36:15 2026        deployed        nginx-25.1.14   1.31.6          Upgrade complete
ubuntu@ip-172-31-6-80:~$ helm rollback my-nginx 1
Rollback was a success! Happy Helming!
ubuntu@ip-172-31-6-80:~$ kubectl get all
NAME                                   READY   STATUS    RESTARTS   AGE
pod/my-nginx-54c75f846f-lbrvp          1/1     Running   0          42m
pod/my-nginx-custom-7c5c88567f-7mwg5   0/1     Pending   0          11m
pod/my-nginx-custom-7c5c88567f-d2cxx   0/1     Pending   0          11m
pod/my-nginx-custom-7c5c88567f-qgtmx   1/1     Running   0          11m

NAME                      TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)                      AGE
service/kubernetes        ClusterIP      10.96.0.1       <none>        443/TCP                      3d15h
service/my-nginx          LoadBalancer   10.96.230.217   <pending>     80:30277/TCP,443:32060/TCP   42m
service/my-nginx-custom   NodePort       10.96.194.64    <none>        80:30216/TCP,443:31670/TCP   11m

NAME                              READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/my-nginx          1/1     1            1           42m
deployment.apps/my-nginx-custom   1/3     3            1           11m

NAME                                         DESIRED   CURRENT   READY   AGE
replicaset.apps/my-nginx-54c75f846f          1         1         1       42m
replicaset.apps/my-nginx-custom-7c5c88567f   3         3         1       11m
ubuntu@ip-172-31-6-80:~$ helm history my-nginx
REVISION        UPDATED                         STATUS          CHART           APP VERSION     DESCRIPTION
1               Tue Sep 22 09:55:16 2026        superseded      nginx-25.1.14   1.31.6          Install complete
2               Tue Sep 22 10:36:15 2026        superseded      nginx-25.1.14   1.31.6          Upgrade complete
3               Tue Sep 22 10:37:49 2026        deployed        nginx-25.1.14   1.31.6          Rollback to 1
ubuntu@ip-172-31-6-80:~$
```

### Verify: How many revisions after the rollback?

**Verify:** After the rollback, there are **3 revisions** in the release history. The rollback creates a new Revision 3 instead of overwriting Revision 2.

## Task 6: Create Your Own Chart

### Scaffold the Helm chart with helm create my-app, explore Chart.yaml, values.yaml, and templates/deployment.yaml along with Go template syntax such as {{ .Values.replicaCount }} and {{ .Chart.Name }}, update values.yaml to set replicaCount to 3 and the image to nginx:1.25, validate the chart with helm lint my-app, preview it using helm template my-release ./my-app, install it with helm install my-release ./my-app, and upgrade it to 5 replicas with helm upgrade my-release ./my-app --set replicaCount=5.

```bash

ubuntu@ip-172-31-6-80:~$ ls
Ecommerce-Website  custom-values.yaml  get_helm.sh  k8s
ubuntu@ip-172-31-6-80:~$ mkdir my-app
ubuntu@ip-172-31-6-80:~$ ls
Ecommerce-Website  custom-values.yaml  get_helm.sh  k8s  my-app
ubuntu@ip-172-31-6-80:~$ cd my-app
ubuntu@ip-172-31-6-80:~/my-app$ ls
ubuntu@ip-172-31-6-80:~/my-app$ helm create my-app
Creating my-app
ubuntu@ip-172-31-6-80:~/my-app$ ls
my-app
ubuntu@ip-172-31-6-80:~/my-app$ cd my-app
ubuntu@ip-172-31-6-80:~/my-app/my-app$ ls
Chart.yaml  charts  templates  values.yaml
ubuntu@ip-172-31-6-80:~/my-app/my-app$ cat Chart.yaml
apiVersion: v2
name: my-app
description: A Helm chart for Kubernetes

# A chart can be either an 'application' or a 'library' chart.
#
# Application charts are a collection of templates that can be packaged into versioned archives
# to be deployed.
#
# Library charts provide useful utilities or functions for the chart developer. They're included as
# a dependency of application charts to inject those utilities and functions into the rendering
# pipeline. Library charts do not define any templates and therefore cannot be deployed.
type: application

# This is the chart version. This version number should be incremented each time you make changes
# to the chart and its templates, including the app version.
# Versions are expected to follow Semantic Versioning (https://semver.org/)
version: 0.1.0

# This is the version number of the application being deployed. This version number should be
# incremented each time you make changes to the application. Versions are not expected to
# follow Semantic Versioning. They should reflect the version the application is using.
# It is recommended to use it with quotes.
appVersion: "1.16.0"
ubuntu@ip-172-31-6-80:~/my-app/my-app$
ubuntu@ip-172-31-6-80:~/my-app/my-app$ cat values
cat: values: No such file or directory
ubuntu@ip-172-31-6-80:~/my-app/my-app$ cat values.yaml
# Default values for my-app.
# This is a YAML-formatted file.
# Declare variables to be passed into your templates.

# This will set the replicaset count more information can be found here: https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/
replicaCount: 1

# This sets the container image more information can be found here: https://kubernetes.io/docs/concepts/containers/images/
image:
  repository: nginx
  # This sets the pull policy for images.
  pullPolicy: IfNotPresent
  # Overrides the image tag whose default is the chart appVersion.
  tag: ""

# This is for the secrets for pulling an image from a private repository more information can be found here: https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/
imagePullSecrets: []
# This is to override the chart name.
nameOverride: ""
fullnameOverride: ""

# This section builds out the service account more information can be found here: https://kubernetes.io/docs/concepts/security/service-accounts/
serviceAccount:
  # Specifies whether a service account should be created.
  create: true
  # Automatically mount a ServiceAccount's API credentials?
  automount: true
  # Annotations to add to the service account.
  annotations: {}
  # The name of the service account to use.
  # If not set and create is true, a name is generated using the fullname template.
  name: ""

# This is for setting Kubernetes Annotations to a Pod.
# For more information checkout: https://kubernetes.io/docs/concepts/overview/working-with-objects/annotations/
podAnnotations: {}
# This is for setting Kubernetes Labels to a Pod.
# For more information checkout: https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/
podLabels: {}

podSecurityContext: {}
  # fsGroup: 2000

securityContext: {}
  # capabilities:
  #   drop:
  #   - ALL
  # readOnlyRootFilesystem: true
  # runAsNonRoot: true
  # runAsUser: 1000

# This is for setting up a service more information can be found here: https://kubernetes.io/docs/concepts/services-networking/service/
service:
  # This sets the service type more information can be found here: https://kubernetes.io/docs/concepts/services-networking/service/#publishing-services-service-types
  type: ClusterIP
  # This sets the ports more information can be found here: https://kubernetes.io/docs/concepts/services-networking/service/#field-spec-ports
  port: 80

# This block is for setting up the ingress for more information can be found here: https://kubernetes.io/docs/concepts/services-networking/ingress/
ingress:
  enabled: false
  className: ""
  annotations: {}
    # kubernetes.io/ingress.class: nginx
    # kubernetes.io/tls-acme: "true"
  hosts:
    - host: chart-example.local
      paths:
        - path: /
          pathType: ImplementationSpecific
  tls: []
    # - secretName: chart-example-tls
    #   hosts:
    #     - chart-example.local

# -- Expose the service via gateway-api HTTPRoute
# Requires Gateway API resources and suitable controller installed within the cluster
# (see: https://gateway-api.sigs.k8s.io/guides/)
httpRoute:
  # HTTPRoute enabled.
  enabled: false
  # HTTPRoute annotations.
  annotations: {}
  # Which Gateways this Route is attached to.
  parentRefs:
  - name: gateway
    sectionName: http
    # namespace: default
  # Hostnames matching HTTP header.
  hostnames:
  - chart-example.local
  # List of rules and filters applied.
  rules:
  - matches:
    - path:
        type: PathPrefix
        value: /headers
  #   filters:
  #   - type: RequestHeaderModifier
  #     requestHeaderModifier:
  #       set:
  #       - name: My-Overwrite-Header
  #         value: this-is-the-only-value
  #       remove:
  #       - User-Agent
  # - matches:
  #   - path:
  #       type: PathPrefix
  #       value: /echo
  #     headers:
  #     - name: version
  #       value: v2

resources: {}
  # We usually recommend not to specify default resources and to leave this as a conscious
  # choice for the user. This also increases chances charts run on environments with little
  # resources, such as Minikube. If you do want to specify resources, uncomment the following
  # lines, adjust them as necessary, and remove the curly braces after 'resources:'.
  # limits:
  #   cpu: 100m
  #   memory: 128Mi
  # requests:
  #   cpu: 100m
  #   memory: 128Mi

# This is to setup the liveness and readiness probes more information can be found here: https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/
livenessProbe:
  httpGet:
    path: /
    port: http
readinessProbe:
  httpGet:
    path: /
    port: http

# This section is for setting up autoscaling more information can be found here: https://kubernetes.io/docs/concepts/workloads/autoscaling/
autoscaling:
  enabled: false
  minReplicas: 1
  maxReplicas: 100
  targetCPUUtilizationPercentage: 80
  # targetMemoryUtilizationPercentage: 80

# Additional volumes on the output Deployment definition.
volumes: []
  # - name: foo
  #   secret:
  #     secretName: mysecret
  #     optional: false

# Additional volumeMounts on the output Deployment definition.
volumeMounts: []
  # - name: foo
  #   mountPath: "/etc/foo"
  #   readOnly: true

nodeSelector: {}

tolerations: []

affinity: {}
ubuntu@ip-172-31-6-80:~/my-app/my-app$
ubuntu@ip-172-31-6-80:~/my-app/my-app$ ls
Chart.yaml  charts  templates  values.yaml
ubuntu@ip-172-31-6-80:~/my-app/my-app$ vim values.yaml
ubuntu@ip-172-31-6-80:~/my-app/my-app$ helm lint my-app
==> Linting my-app
Error unable to check Chart.yaml file in chart: stat my-app/Chart.yaml: no such file or directory

Error: 1 chart(s) linted, 1 chart(s) failed
ubuntu@ip-172-31-6-80:~/my-app/my-app$
ubuntu@ip-172-31-6-80:~/my-app/my-app$
ubuntu@ip-172-31-6-80:~/my-app/my-app$ helm lint .
==> Linting .
[INFO] Chart.yaml: icon is recommended

1 chart(s) linted, 0 chart(s) failed
ubuntu@ip-172-31-6-80:~/my-app/my-app$ helm template my-release ./my-app
Error: path "./my-app" not found
ubuntu@ip-172-31-6-80:~/my-app/my-app$ helm template my-release .
---
# Source: my-app/templates/serviceaccount.yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: my-release-my-app
  labels:
    helm.sh/chart: my-app-0.1.0
    app.kubernetes.io/name: my-app
    app.kubernetes.io/instance: my-release
    app.kubernetes.io/version: "1.16.0"
    app.kubernetes.io/managed-by: Helm
automountServiceAccountToken: true

---
# Source: my-app/templates/service.yaml
apiVersion: v1
kind: Service
metadata:
  name: my-release-my-app
  labels:
    helm.sh/chart: my-app-0.1.0
    app.kubernetes.io/name: my-app
    app.kubernetes.io/instance: my-release
    app.kubernetes.io/version: "1.16.0"
    app.kubernetes.io/managed-by: Helm
spec:
  type: ClusterIP
  ports:
    - port: 80
      targetPort: http
      protocol: TCP
      name: http
  selector:
    app.kubernetes.io/name: my-app
    app.kubernetes.io/instance: my-release

---
# Source: my-app/templates/deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-release-my-app
  labels:
    helm.sh/chart: my-app-0.1.0
    app.kubernetes.io/name: my-app
    app.kubernetes.io/instance: my-release
    app.kubernetes.io/version: "1.16.0"
    app.kubernetes.io/managed-by: Helm
spec:
  replicas: 3
  selector:
    matchLabels:
      app.kubernetes.io/name: my-app
      app.kubernetes.io/instance: my-release
  template:
    metadata:
      labels:
        helm.sh/chart: my-app-0.1.0
        app.kubernetes.io/name: my-app
        app.kubernetes.io/instance: my-release
        app.kubernetes.io/version: "1.16.0"
        app.kubernetes.io/managed-by: Helm
    spec:
      serviceAccountName: my-release-my-app
      containers:
        - name: my-app
          image: "nginx:1.25"
          imagePullPolicy: IfNotPresent
          ports:
            - name: http
              containerPort: 80
              protocol: TCP
          livenessProbe:
            httpGet:
              path: /
              port: http
          readinessProbe:
            httpGet:
              path: /
              port: http
---
# Source: my-app/templates/tests/test-connection.yaml
apiVersion: v1
kind: Pod
metadata:
  name: "my-release-my-app-test-connection"
  labels:
    helm.sh/chart: my-app-0.1.0
    app.kubernetes.io/name: my-app
    app.kubernetes.io/instance: my-release
    app.kubernetes.io/version: "1.16.0"
    app.kubernetes.io/managed-by: Helm
  annotations:
    "helm.sh/hook": test
spec:
  containers:
    - name: wget
      image: busybox
      command: ['wget']
      args: ['my-release-my-app:80']
  restartPolicy: Never

ubuntu@ip-172-31-6-80:~/my-app/my-app$ helm install my-release .
NAME: my-release
LAST DEPLOYED: Tue Sep 22 10:51:44 2026
NAMESPACE: default
STATUS: deployed
REVISION: 1
DESCRIPTION: Install complete
NOTES:
1. Get the application URL by running these commands:
  export POD_NAME=$(kubectl get pods --namespace default -l "app.kubernetes.io/name=my-app,app.kubernetes.io/instance=my-release" -o jsonpath="{.items[0].metadata.name}")
  export CONTAINER_PORT=$(kubectl get pod --namespace default $POD_NAME -o jsonpath="{.spec.containers[0].ports[0].containerPort}")
  echo "Visit http://127.0.0.1:8080 to use your application"
  kubectl --namespace default port-forward $POD_NAME 8080:$CONTAINER_PORT
ubuntu@ip-172-31-6-80:~/my-app/my-app$
ubuntu@ip-172-31-6-80:~/my-app/my-app$ kubectl get all
NAME                                     READY   STATUS    RESTARTS   AGE
pod/my-nginx-54c75f846f-lbrvp            1/1     Running   0          56m
pod/my-nginx-custom-7c5c88567f-7mwg5     0/1     Pending   0          25m
pod/my-nginx-custom-7c5c88567f-d2cxx     0/1     Pending   0          25m
pod/my-nginx-custom-7c5c88567f-qgtmx     1/1     Running   0          25m
pod/my-release-my-app-66b89dff67-6l4ks   1/1     Running   0          27s
pod/my-release-my-app-66b89dff67-jq4nv   1/1     Running   0          27s
pod/my-release-my-app-66b89dff67-r968c   1/1     Running   0          27s

NAME                        TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)                      AGE
service/kubernetes          ClusterIP      10.96.0.1       <none>        443/TCP                      3d15h
service/my-nginx            LoadBalancer   10.96.230.217   <pending>     80:30277/TCP,443:32060/TCP   56m
service/my-nginx-custom     NodePort       10.96.194.64    <none>        80:30216/TCP,443:31670/TCP   25m
service/my-release-my-app   ClusterIP      10.96.45.50     <none>        80/TCP                       27s

NAME                                READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/my-nginx            1/1     1            1           56m
deployment.apps/my-nginx-custom     1/3     3            1           25m
deployment.apps/my-release-my-app   3/3     3            3           27s

NAME                                           DESIRED   CURRENT   READY   AGE
replicaset.apps/my-nginx-54c75f846f            1         1         1       56m
replicaset.apps/my-nginx-custom-7c5c88567f     3         3         1       25m
replicaset.apps/my-release-my-app-66b89dff67   3         3         3       27s
ubuntu@ip-172-31-6-80:~/my-app/my-app$
ubuntu@ip-172-31-6-80:~/my-app/my-app$ helm upgrade my-release ./my-app --set replicaCount=5
Error: path "./my-app" not found
ubuntu@ip-172-31-6-80:~/my-app/my-app$ helm upgrade my-release .  --set replicaCount=5
Release "my-release" has been upgraded. Happy Helming!
NAME: my-release
LAST DEPLOYED: Tue Sep 22 10:53:03 2026
NAMESPACE: default
STATUS: deployed
REVISION: 2
DESCRIPTION: Upgrade complete
NOTES:
1. Get the application URL by running these commands:
  export POD_NAME=$(kubectl get pods --namespace default -l "app.kubernetes.io/name=my-app,app.kubernetes.io/instance=my-release" -o jsonpath="{.items[0].metadata.name}")
  export CONTAINER_PORT=$(kubectl get pod --namespace default $POD_NAME -o jsonpath="{.spec.containers[0].ports[0].containerPort}")
  echo "Visit http://127.0.0.1:8080 to use your application"
  kubectl --namespace default port-forward $POD_NAME 8080:$CONTAINER_PORT
ubuntu@ip-172-31-6-80:~/my-app/my-app$
ubuntu@ip-172-31-6-80:~/my-app/my-app$ kubectl get all
NAME                                     READY   STATUS    RESTARTS   AGE
pod/my-nginx-54c75f846f-lbrvp            1/1     Running   0          57m
pod/my-nginx-custom-7c5c88567f-7mwg5     0/1     Pending   0          26m
pod/my-nginx-custom-7c5c88567f-d2cxx     0/1     Pending   0          26m
pod/my-nginx-custom-7c5c88567f-qgtmx     1/1     Running   0          26m
pod/my-release-my-app-66b89dff67-6l4ks   1/1     Running   0          88s
pod/my-release-my-app-66b89dff67-7jztb   1/1     Running   0          9s
pod/my-release-my-app-66b89dff67-jq4nv   1/1     Running   0          88s
pod/my-release-my-app-66b89dff67-n5qx4   1/1     Running   0          9s
pod/my-release-my-app-66b89dff67-r968c   1/1     Running   0          88s

NAME                        TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)                      AGE
service/kubernetes          ClusterIP      10.96.0.1       <none>        443/TCP                      3d15h
service/my-nginx            LoadBalancer   10.96.230.217   <pending>     80:30277/TCP,443:32060/TCP   57m
service/my-nginx-custom     NodePort       10.96.194.64    <none>        80:30216/TCP,443:31670/TCP   26m
service/my-release-my-app   ClusterIP      10.96.45.50     <none>        80/TCP                       88s

NAME                                READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/my-nginx            1/1     1            1           57m
deployment.apps/my-nginx-custom     1/3     3            1           26m
deployment.apps/my-release-my-app   5/5     5            5           88s

NAME                                           DESIRED   CURRENT   READY   AGE
replicaset.apps/my-nginx-54c75f846f            1         1         1       57m
replicaset.apps/my-nginx-custom-7c5c88567f     3         3         1       26m
replicaset.apps/my-release-my-app-66b89dff67   5         5         5       88s

```
### Verify: After installing, 3 replicas? After upgrading, 5?

Verify: After the initial installation, confirm that 3 replicas are running; after the upgrade, verify that the replica count has increased to 5.

## Task 7: Clean Up

### Uninstall all releases using helm uninstall <name>, remove the chart directory and values file, and use --keep-history when you want to retain the release history for auditing.

```bash

ubuntu@ip-172-31-6-80:~/my-app/my-app/templates$ helm list
NAME            NAMESPACE       REVISION        UPDATED                                 STATUS          CHART           APP VERSION
my-nginx        default         3               2026-09-22 10:37:49.607254643 +0000 UTC deployed        nginx-25.1.14   1.31.6
my-nginx-custom default         1               2026-09-22 10:26:27.346399305 +0000 UTC deployed        nginx-25.1.14   1.31.6
my-release      default         2               2026-09-22 10:53:03.51487526 +0000 UTC  deployed        my-app-0.1.0    1.16.0
ubuntu@ip-172-31-6-80:~/my-app/my-app/templates$ helm uninstall my-release
release "my-release" uninstalled
ubuntu@ip-172-31-6-80:~/my-app/my-app/templates$ helm uninstall my-nginx my-nginx-custom
release "my-nginx" uninstalled

```
### Verify: Does helm list show zero releases?
Verify: Run helm list and confirm that zero releases are listed after uninstalling the Helm releases.
