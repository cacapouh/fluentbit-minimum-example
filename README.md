サンプルアプリのDockerビルド:

```
docker build -t simple-app .
minikube image load simple-app:latest # Minikube環境の場合
```

サンプルアプリのデプロイ:

```
$ kubectl apply -f deployment.yml
deployment.apps/simple-app-deployment created

$ kubectl get pods
NAME                                    READY   STATUS    RESTARTS   AGE
simple-app-deployment-dd44b5ff4-2zhps   1/1     Running   0          5s
simple-app-deployment-dd44b5ff4-58bg4   1/1     Running   0          5s

$ kubectl logs simple-app-deployment-dd44b5ff4-2zhps 
Count 1
Count 2
Count 3
Count 4
Count 5
Count 6
Count 7
Count 8
Count 9
Count 10
Count 11
Count 12
Count 13
Count 14

```

fluent-bitのインストール:

```
$ helm create fluent-bit
Creating fluent-bit

$ cd fluent-bit/templates 
$ rm -rf *

$ helm template fluent-bit fluent/fluent-bit | yq -s '.kind'
$ ls
ClusterRole.yml         ClusterRoleBinding.yml  ConfigMap.yml           DaemonSet.yml           Pod.yml                 Service.yml             ServiceAccount.yml

$ cd ..
$ rm values.yaml

$ cd ..
$ helm install fluent-bid-example ./fluent-bit
NAME: fluent-bid-example
LAST DEPLOYED: Tue Jan  9 21:29:38 2024
NAMESPACE: default
STATUS: deployed
REVISION: 1
```

fluent-bid:

```
$ helm list
NAME                    NAMESPACE       REVISION        UPDATED                                 STATUS          CHART                   APP VERSION
fluent-bid-example      default         1               2024-01-09 21:29:38.763776 +0900 JST    deployed        fluent-bit-0.0.1        1.0.0      

$ kubectl get pods
NAME                                    READY   STATUS    RESTARTS   AGE
fluent-bit-dzqrr                        1/1     Running   0          30s
simple-app-deployment-dd44b5ff4-2zhps   1/1     Running   0          2m13s
simple-app-deployment-dd44b5ff4-58bg4   1/1     Running   0          2m13s

$ kubectl logs fluent-bit-dzqrr | tail      
[0] kube.var.log.containers.simple-app-deployment-dd44b5ff4-2zhps_default_simple-app-8964f46eb8eba5cedbc75be3b8e32c9dbdf182a3f87fd808f82e8e7d19113d30.log: [[1704803524.484566339, {}], {"log"=>"Count 249
", "stream"=>"stdout", "time"=>"2024-01-09T12:32:04.484566339Z"}]
[0] kube.var.log.containers.simple-app-deployment-dd44b5ff4-58bg4_default_simple-app-975041d1a419c97d7d8357b1523fd0cb5c1e2af870bae1a9d43d364a6a15a4fe.log: [[1704803525.484707964, {}], {"log"=>"Count 250
", "stream"=>"stdout", "time"=>"2024-01-09T12:32:05.484707964Z"}]
[0] kube.var.log.containers.simple-app-deployment-dd44b5ff4-2zhps_default_simple-app-8964f46eb8eba5cedbc75be3b8e32c9dbdf182a3f87fd808f82e8e7d19113d30.log: [[1704803525.487212964, {}], {"log"=>"Count 250
", "stream"=>"stdout", "time"=>"2024-01-09T12:32:05.487212964Z"}]
[0] kube.var.log.containers.simple-app-deployment-dd44b5ff4-58bg4_default_simple-app-975041d1a419c97d7d8357b1523fd0cb5c1e2af870bae1a9d43d364a6a15a4fe.log: [[1704803526.488146298, {}], {"log"=>"Count 251
", "stream"=>"stdout", "time"=>"2024-01-09T12:32:06.488146298Z"}]
[0] kube.var.log.containers.simple-app-deployment-dd44b5ff4-2zhps_default_simple-app-8964f46eb8eba5cedbc75be3b8e32c9dbdf182a3f87fd808f82e8e7d19113d30.log: [[1704803526.488614048, {}], {"log"=>"Count 251
", "stream"=>"stdout", "time"=>"2024-01-09T12:32:06.488614048Z"}]
```
