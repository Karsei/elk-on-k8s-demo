# ELK on K8S

## Spec

* Kubernetes v1.25.4 (stable)
* ELK 7.17.4

## Instrument

```bash
# ElasticSearch
$ kubectl apply -f elasticsearch-namespace.yml
$ kubectl apply -f elasticsearch-configmap.yml
$ kubectl apply -f elasticsearch-persistentvolume.yml
$ kubectl apply -f elasticsearch-statefulset.yml
$ kubectl apply -f elasticsearch-service.yml

# Logstash
$ kubectl apply -f logstash-configmap.yml
$ kubectl apply -f logstash-deployment.yml
$ kubectl apply -f logstash-service.yml

# Kibana
$ kubectl apply -f kibana-configmap.yml
$ kubectl apply -f kibana-deployment.yml
$ kubectl apply -f kibana-service.yml
```

```bash
# 확인
$ kubectl get all -n elk
NAME                          READY   STATUS    RESTARTS   AGE
pod/elasticsearch-node-0      1/1     Running   0          10m
pod/kibana-7b777cbccd-gsccf   1/1     Running   0          3m34s

NAME                             TYPE        CLUSTER-IP    EXTERNAL-IP   PORT(S)             AGE
service/elasticsearch            ClusterIP   None          <none>        9200/TCP,9300/TCP   20m
service/elasticsearch-nodeport   NodePort    10.43.39.35   <none>        9200:30920/TCP      20m
service/kibana                   NodePort    10.43.5.9     <none>        33000:30649/TCP     2s

NAME                     READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/kibana   1/1     1            1           3m35s

NAME                                DESIRED   CURRENT   READY   AGE
replicaset.apps/kibana-7b777cbccd   1         1         1       3m35s

NAME                                  READY   AGE
statefulset.apps/elasticsearch-node   1/1     10m

# ElasticSearch 확인
$ curl localhost:30920
{
  "name" : "elasticsearch-node-0",
  "cluster_name" : "elk-cluster",
  "cluster_uuid" : "YFmFbW2ATmaOo_o-JBQwsQ",
  "version" : {
    "number" : "7.17.4",
    "build_flavor" : "default",
    "build_type" : "docker",
    "build_hash" : "79878662c54c886ae89206c685d9f1051a9d6411",
    "build_date" : "2022-05-18T18:04:20.964345128Z",
    "build_snapshot" : false,
    "lucene_version" : "8.11.1",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}

# Kibana 확인
# http://localhost:30649
```

## References

* https://www.skyer9.pe.kr/wordpress/?p=7275
* https://choco-life.tistory.com/55
* https://choco-life.tistory.com/56
* https://github.com/sirzzang/elk-on-k8s-tutorial