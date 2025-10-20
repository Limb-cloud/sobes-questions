# 65 вопросов и ответов по Kubernetes и Helm для Java-разработчиков

## Содержание

### Основы Kubernetes (вопросы 1-15)
1. [Что такое Kubernetes?](#вопрос-1)
2. [Архитектура Kubernetes](#вопрос-2)
3. [Что такое Pod?](#вопрос-3)
4. [Что такое Node?](#вопрос-4)
5. [Что такое Cluster?](#вопрос-5)
6. [Что такое kubectl?](#вопрос-6)
7. [Что такое YAML манифесты?](#вопрос-7)
8. [Что такое Namespace?](#вопрос-8)
9. [Что такое Labels и Selectors?](#вопрос-9)
10. [Что такое Annotations?](#вопрос-10)
11. [Что такое Container и Image?](#вопрос-11)
12. [Что такое Registry?](#вопрос-12)
13. [Жизненный цикл Pod](#вопрос-13)
14. [Что такое Init Containers?](#вопрос-14)
15. [Что такое Sidecar Pattern?](#вопрос-15)

### Workloads и Controllers (вопросы 16-30)
16. [Что такое ReplicaSet?](#вопрос-16)
17. [Что такое Deployment?](#вопрос-17)
18. [Что такое StatefulSet?](#вопрос-18)
19. [Что такое DaemonSet?](#вопрос-19)
20. [Что такое Job и CronJob?](#вопрос-20)
21. [Rolling Updates и Rollbacks](#вопрос-21)
22. [Deployment Strategies](#вопрос-22)
23. [Что такое HPA (Horizontal Pod Autoscaler)?](#вопрос-23)
24. [Что такое VPA (Vertical Pod Autoscaler)?](#вопрос-24)
25. [Resource Requests и Limits](#вопрос-25)
26. [QoS Classes](#вопрос-26)
27. [Pod Disruption Budget](#вопрос-27)
28. [Что такое Liveness, Readiness, Startup Probes?](#вопрос-28)
29. [Pod Affinity и Anti-Affinity](#вопрос-29)
30. [Taints и Tolerations](#вопрос-30)

### Сервисы и сеть (вопросы 31-40)
31. [Что такое Service?](#вопрос-31)
32. [Типы Service (ClusterIP, NodePort, LoadBalancer)](#вопрос-32)
33. [Что такое Ingress?](#вопрос-33)
34. [Service Discovery в Kubernetes](#вопрос-34)
35. [DNS в Kubernetes](#вопрос-35)
36. [NetworkPolicy](#вопрос-36)
37. [CNI (Container Network Interface)](#вопрос-37)
38. [Service Mesh (Istio)](#вопрос-38)
39. [Load Balancing](#вопрос-39)
40. [TLS/SSL в Kubernetes](#вопрос-40)

### Хранилище и конфигурация (вопросы 41-50)
41. [Что такое Volume?](#вопрос-41)
42. [PersistentVolume и PersistentVolumeClaim](#вопрос-42)
43. [StorageClass](#вопрос-43)
44. [ConfigMap](#вопрос-44)
45. [Secret](#вопрос-45)
46. [RBAC (Role-Based Access Control)](#вопрос-46)
47. [Security Context](#вопрос-47)
48. [Мониторинг и логирование](#вопрос-48)
49. [Операторы Kubernetes](#вопрос-49)
50. [Best Practices для Production](#вопрос-50)

### Helm и управление чартами (вопросы 51-65)
51. [Что такое Helm?](#вопрос-51)
52. [Что такое Helm Chart и структура?](#вопрос-52)
53. [Как установить/обновить/удалить Helm chart?](#вопрос-53)
54. [Что такое values.yaml и переопределение значений?](#вопрос-54)
55. [Helm Templates и Go templating](#вопрос-55)
56. [Что такое Helm Hooks?](#вопрос-56)
57. [Как управлять зависимостями в charts?](#вопрос-57)
58. [Helm Repository — работа с репозиториями](#вопрос-58)
59. [Как тестировать и отлаживать charts?](#вопрос-59)
60. [Что такое Helmfile и зачем он нужен?](#вопрос-60)
61. [Различия Helm 2 и Helm 3](#вопрос-61)
62. [Управление секретами в Helm](#вопрос-62)
63. [Helm в CI/CD](#вопрос-63)
64. [Chart Hooks vs. Kubernetes Jobs](#вопрос-64)
65. [Library Chart — как создать?](#вопрос-65)

---

## Вопрос 1

**Что такое Kubernetes?**

Kubernetes — это платформа оркестрации контейнеров с открытым исходным кодом, которая автоматизирует развертывание, масштабирование и управление контейнерными приложениями в кластере машин; обеспечивает декларативное управление через YAML манифесты, self-healing (автоматическое восстановление сбойных компонентов), service discovery, load balancing и rolling updates без downtime.

**Ключевые возможности:**
- **Container Orchestration**: управление lifecycle контейнеров
- **Self-Healing**: автоматический перезапуск failed pods, замена nodes
- **Auto-scaling**: горизонтальное и вертикальное масштабирование
- **Service Discovery**: автоматическое обнаружение сервисов через DNS
- **Load Balancing**: распределение трафика между pods
- **Rolling Updates**: обновления без downtime
- **Secret Management**: безопасное хранение sensitive данных

**Основные концепции:**
```
# Декларативный подход
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: app
        image: my-app:v1.0
```

---

## Вопрос 2

**Архитектура Kubernetes**

Архитектура Kubernetes состоит из Control Plane (управляющие компоненты: API Server, etcd, Scheduler, Controller Manager) и Worker Nodes (kubelet, kube-proxy, container runtime), где Control Plane принимает решения о размещении workloads, а Worker Nodes выполняют actual workloads в контейнерах; все компоненты взаимодействуют через API Server, который является единственной точкой доступа к состоянию кластера в etcd.

**Control Plane компоненты:**

**API Server**: REST API endpoint для всех операций кластера, единственный компонент взаимодействующий с etcd, Authentication/authorization/admission control, Stateless и может быть scaled horizontally.

**etcd**: Distributed key-value store, хранит всё состояние кластера (pods, services, secrets), Strong consistency (CAP theorem: CP), Backup критически важен.

**Scheduler**: Принимает решения о размещении pods на nodes, учитывает resource requirements, node capacity, constraints, Pluggable (можно написать custom scheduler).

**Controller Manager**: Набор controllers (Deployment, ReplicaSet, Node, etc.), Reconciliation loop: desired state → actual state, каждый controller отвечает за свой resource type.

**Worker Node компоненты:**

**kubelet**: "Agent" на каждом node, получает pod specs от API Server, управляет container lifecycle через CRI, отправляет node/pod status обратно в API Server.

**kube-proxy**: Network proxy на каждом node, реализует Service abstraction, Modes: iptables, IPVS, userspace.

**Container Runtime**: Docker, containerd, CRI-O, Pulls images и запускает containers, интерфейс CRI (Container Runtime Interface).

---

## Вопрос 3

**Что такое Pod?**

Pod — это наименьшая deployable единица в Kubernetes, которая содержит один или несколько тесно связанных контейнеров, разделяющих network namespace (IP адрес, порты) и storage volumes; все контейнеры в pod всегда размещаются на одном node, имеют общий lifecycle (создаются/удаляются вместе) и используются для реализации sidecar patterns или тесно связанных компонентов приложения.

**Простой pod манифест:**
```
apiVersion: v1
kind: Pod
metadata:
  name: my-app-pod
  labels:
    app: my-app
spec:
  containers:
  - name: app-container
    image: nginx:1.20
    ports:
    - containerPort: 80
    resources:
      requests:
        memory: "64Mi"
        cpu: "250m"
      limits:
        memory: "128Mi"
        cpu: "500m"
```

**Best practices:** Один pod = одно приложение, используйте multi-container только для тесно связанных компонентов, pods ephemeral — не храните state локально, не создавайте pods напрямую используйте Deployments.

---

## Вопрос 4

**Что такое Node?**

Node — это рабочая машина в Kubernetes кластере (физический сервер или виртуальная машина), на которой запускаются pods и работают компоненты для обеспечения среды выполнения контейнеров; каждый node управляется Control Plane и содержит kubelet (agent для коммуникации с API Server), kube-proxy (для networking), и container runtime для запуска контейнеров.

**Просмотр nodes:**
```
kubectl get nodes                    # Список всех nodes
kubectl describe node <node-name>    # Подробная информация
kubectl top nodes                    # Node capacity и utilization
```

**Cordon/Drain:**
```
kubectl cordon node-1               # Prevent new pods scheduling
kubectl drain node-1 --ignore-daemonsets --delete-emptydir-data
kubectl uncordon node-1             # Enable scheduling обратно
```

---

## Вопрос 5

**Что такое Cluster?**

Cluster — это набор nodes (машин), управляемых единым Control Plane, который предоставляет общие вычислительные ресурсы, сеть и хранилище для запуска контейнерных приложений; кластер абстрагирует индивидуальные машины в единый логический ресурс с общим API, service mesh, shared storage и centralized monitoring/logging.

**Типы кластеров:**

**Development**: minikube, kind (Kubernetes in Docker), k3s (lightweight).

**Production**: AWS EKS, Google GKE, Azure AKS, DigitalOcean DOKS, Self-managed (kubeadm, kops, Rancher, OpenShift).

**Cluster информация:**
```
kubectl cluster-info             # Cluster info
kubectl api-versions            # API versions
kubectl api-resources           # Available resources
kubectl get nodes -o wide       # Cluster nodes
```

---

## Вопрос 6

**Что такое kubectl?**

kubectl — это command-line интерфейс для взаимодействия с Kubernetes API Server, который позволяет создавать, просматривать, модифицировать и удалять ресурсы кластера через declarative (YAML манифесты) или imperative команды; поддерживает работу с multiple кластерами через contexts и предоставляет powerful filtering, output formatting и debugging capabilities.

**Основные операции:**
```
kubectl apply -f manifest.yaml        # Create/Update (declarative)
kubectl get pods                      # View resources
kubectl describe pod my-pod           # Detailed info
kubectl delete pod my-pod             # Delete resources
kubectl logs my-pod -f                # Follow logs
kubectl exec -it my-pod -- bash       # Execute commands
kubectl port-forward pod/my-pod 8080:80  # Port forwarding
```

**Shortcuts:** po=pods, svc=services, deploy=deployments, rs=replicasets, ns=namespaces, no=nodes.

---

## Вопрос 7

**Что такое YAML манифесты?**

YAML манифесты — это декларативные описания желаемого состояния Kubernetes ресурсов в формате YAML, содержащие обязательные поля apiVersion, kind, metadata и spec, где spec определяет desired state ресурса, а Kubernetes controllers автоматически приводят actual state к desired через reconciliation loops; манифесты позволяют версионировать инфраструктуру как код и обеспечивают reproducible deployments.

**Базовая структура:**
```
apiVersion: apps/v1    # API version ресурса
kind: Deployment       # Тип ресурса
metadata:              # Метаданные
  name: my-app
  namespace: default
  labels:
    app: my-app
spec:                  # Desired state
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    spec:
      containers:
      - name: app
        image: my-app:v1.0
```

---

## Вопрос 8

**Что такое Namespace?**

Namespace — это виртуальное разделение единого кластера на множественные логические кластеры для организации и изоляции ресурсов, предоставляющее scope для имён ресурсов (имена должны быть уникальными внутри namespace, но могут повторяться между namespace), а также механизм для применения resource quotas, network policies и RBAC правил на уровне групп ресурсов.

**Default namespaces:** default, kube-system (K8s components), kube-public, kube-node-lease.

**Работа с namespace:**
```
kubectl create namespace production
kubectl get pods -n production
kubectl config set-context --current --namespace=production
```

**Resource Quotas:**
```
apiVersion: v1
kind: ResourceQuota
metadata:
  name: prod-quota
  namespace: production
spec:
  hard:
    requests.cpu: "10"
    requests.memory: 20Gi
    pods: "50"
```

---

## Вопрос 9

**Что такое Labels и Selectors?**

Labels — это key-value пары метаданных, привязанные к Kubernetes ресурсам для их идентификации и группировки, которые используются Selectors для поиска и фильтрации ресурсов; Labels являются основой для Service discovery (Service → Pods), Deployment management (Deployment → ReplicaSet → Pods), и других controller patterns.

**Selectors:**
```
# Equality-based
selector:
  matchLabels:
    app: nginx
    environment: production

# Set-based
selector:
  matchExpressions:
  - key: environment
    operator: In
    values: ["production", "staging"]
```

**kubectl с labels:**
```
kubectl get pods -l app=nginx
kubectl label pods my-pod version=v1.0
kubectl label pods my-pod version=v2.0 --overwrite
```

---

## Вопрос 10

**Что такое Annotations?**

Annotations — это key-value метаданные в Kubernetes ресурсах, предназначенные для хранения произвольной информации, которая НЕ используется для идентификации и selection ресурсов (в отличие от Labels), а служит для передачи конфигурационных данных внешним инструментам, controllers, операторам и системам мониторинга.

**Примеры annotations:**
```
metadata:
  annotations:
    kubernetes.io/change-cause: "Updated to version 1.20"
    nginx.ingress.kubernetes.io/rewrite-target: /
    prometheus.io/scrape: "true"
    prometheus.io/port: "8080"
```

**Use cases:** Tool configuration (Ingress controllers, service mesh, monitoring), Build metadata (commit hash, build number), Business metadata (cost center, project), Operational notes.

---

## Вопрос 11

**Что такое Container и Image?**

Container — это изолированный процесс со своей файловой системой, network namespace и resource limits, созданный из Container Image; Image — это read-only template с application code, runtime, libraries и dependencies, построенный layers и хранящийся в Container Registry; в Kubernetes containers запускаются внутри Pods и управляются через Container Runtime Interface (CRI).

**Container spec:**
```
spec:
  containers:
  - name: app
    image: my-app:v1.2.3
    imagePullPolicy: IfNotPresent
    ports:
    - containerPort: 8080
    resources:
      requests:
        memory: "256Mi"
        cpu: "250m"
      limits:
        memory: "512Mi"
        cpu: "500m"
```

---

## Вопрос 12

**Что такое Registry?**

Registry — это централизованное хранилище Container Images, которое предоставляет API для push, pull и management images с поддержкой версионирования через tags, access control, vulnerability scanning и integration с CI/CD pipelines; Kubernetes kubelet pull'ит images из registries при создании containers.

**ImagePullSecrets:**
```
kubectl create secret docker-registry my-registry-secret \
  --docker-server=my-registry.com \
  --docker-username=myuser \
  --docker-password=mypassword
```

```
spec:
  imagePullSecrets:
  - name: my-registry-secret
  containers:
  - name: app
    image: my-registry.com/my-app:v1.0
```

---

## Вопрос 13

**Жизненный цикл Pod**

Жизненный цикл Pod проходит через фазы Pending → Running → Succeeded/Failed, где Kubernetes scheduler назначает Pod на Node, kubelet запускает containers через Container Runtime.

**Фазы:** Pending (scheduled но не started), Running (хотя бы один container запущен), Succeeded (все containers завершились успешно), Failed (хотя бы один container failed), Unknown (состояние не может быть определено).

**Restart Policies:**
```
spec:
  restartPolicy: Always      # Default
  restartPolicy: OnFailure   # Restart только при exit code != 0
  restartPolicy: Never       # Никогда не restart
```

---

## Вопрос 14

**Что такое Init Containers?**

Init Containers — это специальные контейнеры, которые запускаются и завершаются до запуска основных контейнеров в Pod; выполняются последовательно (один за другим) и все должны завершиться успешно (exit code 0); используются для initialization tasks, prerequisites, setup scripts.

**Пример:**
```
spec:
  initContainers:
  - name: wait-for-db
    image: busybox
    command: ['sh', '-c', 'until nc -z database-service 5432; do sleep 2; done']
  containers:
  - name: main-app
    image: my-app:v1.0
```

---

## Вопрос 15

**Что такое Sidecar Pattern?**

Sidecar Pattern — это архитектурный паттерн, где дополнительный контейнер (sidecar) размещается в том же Pod что и основное приложение для предоставления вспомогательных функций: логирование, мониторинг, proxy, security; sidecar разделяет network, storage и lifecycle с main container.

**Пример:**
```
spec:
  containers:
  - name: main-app
    image: my-app:v1.0
  - name: log-shipper
    image: fluentd:latest
    volumeMounts:
    - name: log-volume
      mountPath: /var/log
```

---

## Вопрос 16

**Что такое ReplicaSet?**

ReplicaSet — это Kubernetes controller, который обеспечивает запуск указанного количества идентичных pod replicas в любой момент времени, автоматически создавая новые pods при их удалении или падении и удаляя лишние при превышении desired count; использует label selectors для идентификации управляемых pods и не рекомендуется создавать напрямую (используйте Deployment вместо этого).

**Манифест:**
```
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: nginx-replicaset
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.20
```

**Команды:**
```
kubectl get rs                    # Список ReplicaSets
kubectl scale rs nginx-replicaset --replicas=5
kubectl describe rs nginx-replicaset
```

---

## Вопрос 17

**Что такое Deployment?**

Deployment — это high-level Kubernetes controller, который управляет ReplicaSets и обеспечивает декларативные updates для Pods, поддерживая rolling updates (постепенное обновление pods без downtime), rollbacks (откат к предыдущим версиям), scaling (изменение количества replicas), и pause/resume для batch changes; Deployment создаёт новый ReplicaSet при изменении pod template и постепенно переключает трафик с old на new ReplicaSet.

**Манифест:**
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.20
        ports:
        - containerPort: 80
```

**Операции:**
```
kubectl apply -f deployment.yaml
kubectl set image deployment/nginx-deployment nginx=nginx:1.21
kubectl rollout status deployment/nginx-deployment
kubectl rollout history deployment/nginx-deployment
kubectl rollout undo deployment/nginx-deployment
kubectl scale deployment nginx-deployment --replicas=5
```

---

## Вопрос 18

**Что такое StatefulSet?**

StatefulSet — это Kubernetes controller для управления stateful приложениями, который обеспечивает stable network identity (предсказуемые DNS имена), persistent storage (каждый pod получает свой PersistentVolumeClaim), ordered deployment и scaling (pods создаются/удаляются последовательно от 0 до N-1), и используется для баз данных, distributed systems, applications требующих stable storage и network identity.

**Манифест:**
```
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: postgres
spec:
  serviceName: postgres-headless
  replicas: 3
  selector:
    matchLabels:
      app: postgres
  template:
    metadata:
      labels:
        app: postgres
    spec:
      containers:
      - name: postgres
        image: postgres:15
        ports:
        - containerPort: 5432
        volumeMounts:
        - name: data
          mountPath: /var/lib/postgresql/data
  volumeClaimTemplates:
  - metadata:
      name: data
    spec:
      accessModes: ["ReadWriteOnce"]
      resources:
        requests:
          storage: 10Gi
```

**Pod naming:** `<statefulset-name>-<ordinal>` (postgres-0, postgres-1, postgres-2).

**DNS:** `<pod-name>.<service-name>.<namespace>.svc.cluster.local` (postgres-0.postgres-headless.default.svc.cluster.local).

---

## Вопрос 19

**Что такое DaemonSet?**

DaemonSet — это Kubernetes controller, который обеспечивает запуск ровно одной копии pod на каждом (или выбранном через nodeSelector/affinity) node в кластере, автоматически добавляя pod на новые nodes и удаляя при удалении nodes; используется для cluster-wide services: logging agents (Fluentd), monitoring (Node Exporter), network plugins (Calico), storage daemons.

**Манифест:**
```
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: fluentd
spec:
  selector:
    matchLabels:
      app: fluentd
  template:
    metadata:
      labels:
        app: fluentd
    spec:
      containers:
      - name: fluentd
        image: fluentd:v1.14
        volumeMounts:
        - name: varlog
          mountPath: /var/log
      volumes:
      - name: varlog
        hostPath:
          path: /var/log
```

**Use cases:** Log collection на каждом node, Node monitoring, CNI network plugin, Storage daemon, Security agent.

---

## Вопрос 20

**Что такое Job и CronJob?**

Job — это Kubernetes controller для запуска batch/одноразовых tasks до успешного завершения (один или несколько pods), который создаёт pods и отслеживает их completion, поддерживает parallelism (одновременный запуск N pods) и completions (сколько успешных завершений требуется); CronJob — это wrapper над Job для запуска по расписанию (cron-like syntax), создающий новый Job на каждое срабатывание schedule.

**Job:**
```
apiVersion: batch/v1
kind: Job
metadata:
  name: data-migration
spec:
  completions: 1
  parallelism: 1
  backoffLimit: 3
  template:
    spec:
      restartPolicy: Never
      containers:
      - name: migrator
        image: my-migrator:v1.0
        command: ["python", "migrate.py"]
```

**CronJob:**
```
apiVersion: batch/v1
kind: CronJob
metadata:
  name: backup
spec:
  schedule: "0 2 * * *"  # Каждый день в 2:00
  jobTemplate:
    spec:
      template:
        spec:
          restartPolicy: OnFailure
          containers:
          - name: backup
            image: backup-tool:v1.0
            command: ["./backup.sh"]
```

---

## Вопрос 21

**Rolling Updates и Rollbacks**

Rolling Updates — это стратегия обновления Deployment, при которой pods постепенно заменяются новой версией без полного downtime, контролируя maxSurge (сколько дополнительных pods можно создать) и maxUnavailable (сколько pods могут быть unavailable); Rollback — это откат к предыдущей версии Deployment через сохранённые ReplicaSets, сохраняющий историю revisions (по умолчанию последние 10).

**Rolling Update конфигурация:**
```
spec:
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 25%           # Max 25% дополнительных pods
      maxUnavailable: 25%     # Max 25% unavailable pods
```

**Команды:**
```
kubectl set image deployment/app app=app:v2.0 --record
kubectl rollout status deployment/app
kubectl rollout history deployment/app
kubectl rollout undo deployment/app              # Откат к предыдущей
kubectl rollout undo deployment/app --to-revision=3
kubectl rollout pause deployment/app             # Приостановить rollout
kubectl rollout resume deployment/app
```

---

## Вопрос 22

**Deployment Strategies**

Deployment Strategies определяют как выполняется обновление приложения: RollingUpdate (постепенная замена pods, default, zero downtime), Recreate (удаление всех old pods перед созданием new, downtime есть), Blue-Green (полностью новая версия деплоится параллельно, переключение трафика мгновенное), Canary (новая версия получает малую часть трафика для тестирования), A/B Testing (маршрутизация по признакам пользователя).

**Recreate:**
```
spec:
  strategy:
    type: Recreate  # Все pods удаляются, затем создаются новые
```

**Blue-Green (через Service selector):**
```
# Blue deployment (current)
apiVersion: apps/v1
kind: Deployment
metadata:
  name: app-blue
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
      version: blue

***
# Green deployment (new)
apiVersion: apps/v1
kind: Deployment
metadata:
  name: app-green
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
      version: green

***
# Service переключается изменением selector
apiVersion: v1
kind: Service
spec:
  selector:
    app: myapp
    version: blue  # Меняем на green для переключения
```

**Canary:**
```
# Stable version (90% traffic)
spec:
  replicas: 9
  selector:
    matchLabels:
      app: myapp
      track: stable

# Canary version (10% traffic)
spec:
  replicas: 1
  selector:
    matchLabels:
      app: myapp
      track: canary
```

---

## Вопрос 23

**Что такое HPA (Horizontal Pod Autoscaler)?**

HPA — это Kubernetes controller, который автоматически масштабирует количество pod replicas в Deployment/ReplicaSet/StatefulSet на основе наблюдаемых метрик (CPU utilization, memory, custom metrics из Prometheus), периодически проверяя metrics (default каждые 15 секунд) и adjusting replicas для достижения target utilization; требует установленного Metrics Server и resource requests в pod specs.

**Манифест:**
```
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: app-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: my-app
  minReplicas: 2
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 70
  - type: Resource
    resource:
      name: memory
      target:
        type: Utilization
        averageUtilization: 80
  behavior:
    scaleDown:
      stabilizationWindowSeconds: 300
      policies:
      - type: Percent
        value: 50
        periodSeconds: 60
```

**Создание через kubectl:**
```
kubectl autoscale deployment my-app --cpu-percent=70 --min=2 --max=10
kubectl get hpa
kubectl describe hpa app-hpa
```

---

## Вопрос 24

**Что такое VPA (Vertical Pod Autoscaler)?**

VPA — это Kubernetes controller, который автоматически adjusts CPU и memory requests/limits для containers на основе исторического использования и real-time metrics, рекомендуя или автоматически applying оптимальные resource values; работает в трёх режимах: Off (только recommendations), Initial (устанавливает при создании), Auto (обновляет через pod restart); не совместим с HPA на CPU/memory metrics.

**Манифест:**
```
apiVersion: autoscaling.k8s.io/v1
kind: VerticalPodAutoscaler
metadata:
  name: app-vpa
spec:
  targetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: my-app
  updatePolicy:
    updateMode: "Auto"  # Off, Initial, Auto
  resourcePolicy:
    containerPolicies:
    - containerName: app
      minAllowed:
        cpu: 100m
        memory: 128Mi
      maxAllowed:
        cpu: 2
        memory: 2Gi
```

**Режимы:** Off (recommendations only, no changes), Initial (set на pod creation), Auto (update через recreation), Recreate (evict и recreate pods).

---

## Вопрос 25

**Resource Requests и Limits**

Resource Requests — это гарантированное количество CPU/memory, которое Kubernetes scheduler резервирует на node для pod (используется для scheduling decisions); Limits — это максимальное количество ресурсов, которое container может использовать (при превышении memory — pod killed OOMKilled, CPU — throttled); requests ≤ actual usage ≤ limits, правильная установка критична для QoS classes и cluster stability.

**Спецификация:**
```
spec:
  containers:
  - name: app
    image: my-app:v1.0
    resources:
      requests:
        memory: "256Mi"   # Гарантированный минимум
        cpu: "250m"       # 250 milliCPU (0.25 CPU)
      limits:
        memory: "512Mi"   # Максимум
        cpu: "500m"       # При превышении — throttling
```

**CPU units:** 1 CPU = 1000m (millicores), 0.5 CPU = 500m, можно указывать как "0.5" или "500m".

**Memory units:** Ki (kibibyte), Mi (mebibyte), Gi (gibibyte), или K, M, G (decimal).

**Best practices:**
- Всегда устанавливайте requests (для правильного scheduling)
- Limits опциональны но рекомендуются
- requests = limits для Guaranteed QoS
- Мониторьте actual usage (kubectl top pods)
- Не устанавливайте слишком высокие limits (ресурсы waste)

---

## Вопрос 26

**QoS Classes**

QoS (Quality of Service) Classes — это классификация pods в Kubernetes на основе resource requests/limits, определяющая приоритет eviction при resource pressure на node: Guaranteed (highest priority, requests = limits для всех containers), Burstable (средний приоритет, requests < limits или requests без limits), BestEffort (lowest priority, нет requests и limits, evicted первыми); QoS class назначается автоматически и влияет на OOM killer order.

**Guaranteed:**
```
# requests = limits для ВСЕХ containers и ВСЕ ресурсы указаны
resources:
  requests:
    memory: "256Mi"
    cpu: "500m"
  limits:
    memory: "256Mi"
    cpu: "500m"
```

**Burstable:**
```
# requests < limits ИЛИ только requests указаны
resources:
  requests:
    memory: "128Mi"
    cpu: "250m"
  limits:
    memory: "512Mi"
    cpu: "1"
```

**BestEffort:**
```
# Нет requests и limits вообще
# Evicted первыми при resource pressure
```

**Eviction order:** BestEffort (первыми) → Burstable (по usage relative to requests) → Guaranteed (последними, только при system критично).

---

## Вопрос 27

**Pod Disruption Budget**

Pod Disruption Budget (PDB) — это Kubernetes resource, который ограничивает количество одновременно unavailable pods во время voluntary disruptions (node drain, deployment updates, cluster upgrades), указывая minAvailable (минимум доступных pods) или maxUnavailable (максимум недоступных); не защищает от involuntary disruptions (hardware failures, kernel panic) и используется для обеспечения application availability во время maintenance.

**Манифест:**
```
apiVersion: policy/v1
kind: PodDisruptionBudget
metadata:
  name: app-pdb
spec:
  minAvailable: 2              # Минимум 2 pods всегда available
  # ИЛИ
  maxUnavailable: 1            # Максимум 1 pod может быть unavailable
  selector:
    matchLabels:
      app: my-app
```

**Use cases:** Предотвращение одновременного eviction всех pods при node drain, обеспечение quorum для distributed systems, защита критичных сервисов от downtime.

**Проверка:**
```
kubectl get pdb
kubectl describe pdb app-pdb
```

---

## Вопрос 28

**Что такое Liveness, Readiness, Startup Probes?**

Probes — это health checks выполняемые kubelet для определения состояния containers: Liveness Probe проверяет жив ли container (restart при failure), Readiness Probe определяет готов ли pod принимать трафик (исключает из Service endpoints при failure), Startup Probe защищает slow-starting containers от преждевременных liveness checks (отключает liveness/readiness до успеха startup); поддерживают три handler types: httpGet, tcpSocket, exec.

**Liveness Probe:**
```
livenessProbe:
  httpGet:
    path: /healthz
    port: 8080
  initialDelaySeconds: 30   # Задержка перед первой проверкой
  periodSeconds: 10         # Частота проверок
  timeoutSeconds: 5         # Timeout на запрос
  failureThreshold: 3       # Сколько failures до restart
```

**Readiness Probe:**
```
readinessProbe:
  httpGet:
    path: /ready
    port: 8080
  initialDelaySeconds: 5
  periodSeconds: 5
  successThreshold: 1       # Сколько successes до ready
  failureThreshold: 3
```

**Startup Probe:**
```
startupProbe:
  httpGet:
    path: /startup
    port: 8080
  failureThreshold: 30      # 30 * 10s = 5 минут на startup
  periodSeconds: 10
```

**Handler types:**
```
# HTTP GET
httpGet:
  path: /health
  port: 8080
  httpHeaders:
  - name: Custom-Header
    value: Value

# TCP Socket
tcpSocket:
  port: 8080

# Command
exec:
  command:
  - cat
  - /tmp/healthy
```

---

## Вопрос 29

**Pod Affinity и Anti-Affinity**

Pod Affinity/Anti-Affinity — это правила размещения pods относительно других pods на основе labels, где Affinity притягивает pods к nodes с определёнными pods (co-locate related services), Anti-Affinity отталкивает pods от nodes с определёнными pods (spread replicas для HA); поддерживают required (hard constraint, must) и preferred (soft constraint, best effort) rules с topologyKey определяющим scope (node, zone, region).

**Pod Anti-Affinity (spread replicas):**
```
spec:
  affinity:
    podAntiAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
      - labelSelector:
          matchExpressions:
          - key: app
            operator: In
            values:
            - my-app
        topologyKey: kubernetes.io/hostname  # Разные nodes

      preferredDuringSchedulingIgnoredDuringExecution:
      - weight: 100
        podAffinityTerm:
          labelSelector:
            matchLabels:
              app: my-app
          topologyKey: topology.kubernetes.io/zone  # Prefer разные zones
```

**Pod Affinity (co-locate):**
```
spec:
  affinity:
    podAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
      - labelSelector:
          matchLabels:
            app: cache
        topologyKey: kubernetes.io/hostname  # На том же node что cache
```

**topologyKey:** kubernetes.io/hostname (node-level), topology.kubernetes.io/zone (zone-level), topology.kubernetes.io/region (region-level).

---

## Вопрос 30

**Taints и Tolerations**

Taints — это метки на nodes отталкивающие pods от scheduling (node "помечен"), Tolerations — это разрешения в pod specs позволяющие scheduling на tainted nodes; вместе реализуют node restrictions: dedicated nodes (только определённые workloads), special hardware (GPU nodes), node maintenance, node problems (disk pressure); taint имеет key, value, effect (NoSchedule, PreferNoSchedule, NoExecute).

**Taint на node:**
```
kubectl taint nodes node-1 key=value:NoSchedule
kubectl taint nodes node-1 key=value:NoSchedule-  # Remove taint
kubectl taint nodes node-1 gpu=true:NoSchedule
```

**Toleration в pod:**
```
spec:
  tolerations:
  - key: "key"
    operator: "Equal"
    value: "value"
    effect: "NoSchedule"
  
  # Tolerate все taints с key=gpu
  - key: "gpu"
    operator: "Exists"
    effect: "NoSchedule"
```

**Effects:**
- **NoSchedule**: новые pods не будут scheduled (existing остаются)
- **PreferNoSchedule**: avoid scheduling но не запрещено
- **NoExecute**: evict existing pods без toleration

**Use cases:** Dedicated nodes для specific workloads, GPU/special hardware nodes, Node maintenance (cordoned nodes), Automatic taints (node.kubernetes.io/not-ready, node.kubernetes.io/unreachable).

---

## Вопрос 31

**Что такое Service?**

Service — это Kubernetes abstraction определяющая логический набор pods (через label selector) и policy доступа к ним, предоставляя stable IP address и DNS name независимо от pod lifecycle (pods могут умирать и пересоздаваться, Service IP остаётся); реализует load balancing между pods, service discovery через DNS (service-name.namespace.svc.cluster.local), и поддерживает различные типы: ClusterIP, NodePort, LoadBalancer, ExternalName.

**Манифест:**
```
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: my-app        # Pods с этими labels
  ports:
  - name: http
    protocol: TCP
    port: 80           # Service port
    targetPort: 8080   # Container port
  type: ClusterIP      # Default type
```

**Service Discovery:**
```
# DNS внутри кластера
curl http://my-service                                    # Same namespace
curl http://my-service.default                            # Explicit namespace
curl http://my-service.default.svc.cluster.local         # FQDN

# Environment variables (legacy)
MY_SERVICE_SERVICE_HOST=10.96.0.1
MY_SERVICE_SERVICE_PORT=80
```

---

## Вопрос 32

**Типы Service (ClusterIP, NodePort, LoadBalancer)**

Service типы определяют способ expose приложения: ClusterIP (default, internal cluster IP, доступен только внутри кластера), NodePort (открывает порт на всех nodes, доступен извне как NodeIP:NodePort), LoadBalancer (создаёт external load balancer в cloud provider, получает external IP), ExternalName (CNAME record к внешнему DNS); выбор зависит от требований доступности (internal vs external) и инфраструктуры (cloud vs on-premise).

**ClusterIP:**
```
spec:
  type: ClusterIP      # Default, только internal access
  clusterIP: 10.96.0.10  # Можно указать или auto-assigned
  ports:
  - port: 80
    targetPort: 8080
```

**NodePort:**
```
spec:
  type: NodePort
  ports:
  - port: 80
    targetPort: 8080
    nodePort: 30080    # 30000-32767 range, auto-assigned если не указан
# Доступ: <NodeIP>:30080
```

**LoadBalancer:**
```
spec:
  type: LoadBalancer   # Создаёт external LB (AWS ELB, GCP LB, Azure LB)
  ports:
  - port: 80
    targetPort: 8080
# Получает external IP от cloud provider
```

**ExternalName:**
```
spec:
  type: ExternalName
  externalName: external-service.example.com  # CNAME
# Внутренние запросы к my-service → external-service.example.com
```

**Headless Service (ClusterIP: None):**
```
spec:
  clusterIP: None      # Нет load balancing, direct pod IPs
  selector:
    app: my-app
# DNS возвращает IP всех pods напрямую
```

---

## Вопрос 33

**Что такое Ingress?**

Ingress — это Kubernetes API object управляющий external HTTP/HTTPS access к services в кластере, предоставляя host-based и path-based routing, SSL/TLS termination, name-based virtual hosting, и требующий Ingress Controller (nginx, traefik, HAProxy, istio) для actual implementation; позволяет expose multiple services через single external IP с гибкими правилами маршрутизации.

**Манифест:**
```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-ingress
  annotations:
    kubernetes.io/ingress.class: nginx
    nginx.ingress.kubernetes.io/rewrite-target: /
    cert-manager.io/cluster-issuer: letsencrypt-prod
spec:
  tls:
  - hosts:
    - example.com
    secretName: tls-secret
  rules:
  - host: example.com
    http:
      paths:
      - path: /app
        pathType: Prefix
        backend:
          service:
            name: app-service
            port:
              number: 80
      - path: /api
        pathType: Prefix
        backend:
          service:
            name: api-service
            port:
              number: 8080
  - host: blog.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: blog-service
            port:
              number: 80
```

**Path types:** Exact (точное совпадение), Prefix (совпадение префикса), ImplementationSpecific (зависит от Ingress Controller).

---

## Вопрос 34

**Service Discovery в Kubernetes**

Service Discovery — это механизм automatic обнаружения services в кластере через DNS (рекомендуется, CoreDNS предоставляет DNS records для всех services) или Environment Variables (legacy, injected в pods при creation с service host/port); позволяет pods находить и connect к services по имени без hardcoded IP addresses, работает для ClusterIP services и поддерживает headless services для direct pod access.

**DNS-based (рекомендуется):**
```
# Формат: <service-name>.<namespace>.svc.cluster.local

# Same namespace
curl http://my-service

# Cross-namespace
curl http://my-service.production

# FQDN
curl http://my-service.production.svc.cluster.local

# Headless service (возвращает pod IPs)
curl http://my-statefulset-0.my-headless-service
```

**Environment Variables:**
```
# Автоматически injected для services существующих при pod creation
MY_SERVICE_SERVICE_HOST=10.96.0.1
MY_SERVICE_SERVICE_PORT=80
MY_SERVICE_PORT=tcp://10.96.0.1:80
MY_SERVICE_PORT_80_TCP=tcp://10.96.0.1:80
MY_SERVICE_PORT_80_TCP_PROTO=tcp
MY_SERVICE_PORT_80_TCP_PORT=80
MY_SERVICE_PORT_80_TCP_ADDR=10.96.0.1
```

---

## Вопрос 35

**DNS в Kubernetes**

DNS в Kubernetes предоставляется CoreDNS (cluster add-on), который автоматически создаёт DNS A/AAAA records для services и pods, поддерживает поиск по short names (same namespace) и FQDN, предоставляет SRV records для named ports, и настраивается через dnsPolicy и dnsConfig в pod specs; каждый pod получает /etc/resolv.conf с nameserver указывающим на CoreDNS service.

**DNS схема:**

**Services:**
```
<service-name>.<namespace>.svc.<cluster-domain>
my-service.default.svc.cluster.local → ClusterIP

<service-name>.<namespace>
my-service.default

<service-name> (same namespace только)
my-service
```

**Pods:**
```
<pod-ip-with-dashes>.<namespace>.pod.<cluster-domain>
10-244-1-5.default.pod.cluster.local → Pod IP
```

**Headless Service Pods:**
```
<pod-name>.<service-name>.<namespace>.svc.<cluster-domain>
postgres-0.postgres-headless.default.svc.cluster.local
```

**dnsPolicy:**
```
spec:
  dnsPolicy: ClusterFirst    # Default, использует CoreDNS
  # ClusterFirstWithHostNet  # For pods with hostNetwork: true
  # Default                  # Inherit from node
  # None                     # Custom dnsConfig
```

**Custom DNS:**
```
spec:
  dnsPolicy: None
  dnsConfig:
    nameservers:
    - 8.8.8.8
    searches:
    - my-namespace.svc.cluster.local
    - svc.cluster.local
    - cluster.local
    options:
    - name: ndots
      value: "2"
```

## Вопрос 36

**NetworkPolicy**

NetworkPolicy — это Kubernetes specification для управления сетевым трафиком на IP/port уровне между pods, namespaces и external endpoints, действующая как distributed firewall через label selectors и реализуемая CNI plugins (Calico, Cilium, Weave); по умолчанию все pods могут communicate между собой (allow-all), NetworkPolicy создаёт deny-all правило с explicit allow rules для ingress/egress трафика, поддерживает namespace isolation и micro-segmentation.

**Deny all ingress:**
```
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: deny-all-ingress
  namespace: production
spec:
  podSelector: {}    # Применяется ко всем pods в namespace
  policyTypes:
  - Ingress
  # Нет ingress rules = deny all
```

**Allow specific ingress:**
```
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-frontend
  namespace: production
spec:
  podSelector:
    matchLabels:
      app: backend    # К каким pods применяется
  policyTypes:
  - Ingress
  ingress:
  - from:
    - podSelector:
        matchLabels:
          app: frontend    # От каких pods разрешено
    - namespaceSelector:
        matchLabels:
          env: production  # От pods в namespaces с label
    ports:
    - protocol: TCP
      port: 8080
```

**Egress policy:**
```
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-dns-egress
spec:
  podSelector:
    matchLabels:
      app: my-app
  policyTypes:
  - Egress
  egress:
  - to:
    - namespaceSelector:
        matchLabels:
          name: kube-system
    ports:
    - protocol: UDP
      port: 53    # DNS
  - to:
    - podSelector:
        matchLabels:
          app: database
    ports:
    - protocol: TCP
      port: 5432
```

**Allow from external CIDR:**
```
egress:
- to:
  - ipBlock:
      cidr: 10.0.0.0/24
      except:
      - 10.0.0.5/32
  ports:
  - protocol: TCP
    port: 443
```

**Best practices:** Default deny-all, explicit allow needed services, namespace isolation, label-based rules для flexibility, тестируйте перед production, требует CNI plugin support.

---

## Вопрос 37

**CNI (Container Network Interface)**

CNI — это specification и plugins для конфигурирования network interfaces в Linux containers, определяющий как container runtimes должны настраивать networking; в Kubernetes CNI plugins обеспечивают pod-to-pod communication, IP address management, NetworkPolicy enforcement, и выбираются при cluster creation: Flannel (простой overlay), Calico (BGP-based, NetworkPolicy), Cilium (eBPF-based, advanced features), Weave Net (mesh networking).

**CNI функции:**
- **Pod networking**: каждый pod получает IP address
- **Network isolation**: namespace/pod level через NetworkPolicy
- **Service mesh integration**: Istio, Linkerd compatible
- **IPAM**: IP address management
- **Multi-tenancy**: network separation между teams

**Популярные CNI:**

**Flannel (простой):**
- Overlay network (VXLAN)
- Простая конфигурация
- Нет NetworkPolicy support
- Use case: simple clusters, development

**Calico (production):**
- BGP routing или IP-in-IP
- NetworkPolicy enforcement
- eBPF dataplane опция
- Use case: production, enterprise

**Cilium (advanced):**
- eBPF-based (kernel-level)
- L7 NetworkPolicy (HTTP-level)
- Service mesh без sidecars
- Use case: advanced networking, observability

**Weave Net:**
- Mesh networking
- Automatic network discovery
- Encryption support
- Use case: multi-cloud

**Проверка CNI:**
```
# Какой CNI используется
kubectl get pods -n kube-system | grep -E 'calico|flannel|cilium|weave'

# CNI config на node
cat /etc/cni/net.d/
```

---

## Вопрос 38

**Service Mesh (Istio)**

Service Mesh — это dedicated infrastructure layer для управления service-to-service communication в microservices через sidecar proxies (Envoy), предоставляя traffic management (routing, retries, timeouts), security (mTLS, authorization), observability (metrics, tracing, logs) без изменения application code; Istio — популярная реализация добавляющая control plane (istiod) и data plane (Envoy sidecars) для advanced networking features.

**Istio архитектура:**
```
Application Pod:
├── App Container (port 8080)
└── Envoy Sidecar Proxy (intercepts all traffic)
         ↓
    Istio Control Plane (istiod)
    - Configuration
    - Certificate management
    - Service discovery
```

**Возможности:**

**Traffic Management:**
```
apiVersion: networking.istio.io/v1beta1
kind: VirtualService
metadata:
  name: my-service
spec:
  hosts:
  - my-service
  http:
  - match:
    - headers:
        canary:
          exact: "true"
    route:
    - destination:
        host: my-service
        subset: v2
      weight: 100
  - route:
    - destination:
        host: my-service
        subset: v1
      weight: 90
    - destination:
        host: my-service
        subset: v2
      weight: 10  # Canary 10%
```

**Circuit Breaking:**
```
apiVersion: networking.istio.io/v1beta1
kind: DestinationRule
metadata:
  name: my-service
spec:
  host: my-service
  trafficPolicy:
    connectionPool:
      tcp:
        maxConnections: 100
      http:
        http1MaxPendingRequests: 10
        maxRequestsPerConnection: 2
    outlierDetection:
      consecutiveErrors: 5
      interval: 30s
      baseEjectionTime: 30s
```

**mTLS:**
```
apiVersion: security.istio.io/v1beta1
kind: PeerAuthentication
metadata:
  name: default
spec:
  mtls:
    mode: STRICT  # Enforce mTLS для всех services
```

**Установка:**
```
# Istio CLI
istioctl install --set profile=demo

# Enable sidecar injection для namespace
kubectl label namespace default istio-injection=enabled

# Verify
kubectl get pods -n istio-system
```

**Use cases:** Microservices communication, Traffic splitting (canary/blue-green), Circuit breaking, mTLS между services, Distributed tracing, Metrics/observability.

---

## Вопрос 39

**Load Balancing**

Load Balancing в Kubernetes реализуется на нескольких уровнях: kube-proxy (L4, Service load balancing между pods через iptables/IPVS), Ingress Controller (L7, HTTP/HTTPS load balancing с host/path routing), External Load Balancer (cloud provider LB для Service type LoadBalancer), Service Mesh (Envoy sidecar с advanced algorithms); выбор зависит от протокола (HTTP vs TCP), требований (session affinity, health checks) и окружения (cloud vs on-premise).

**Service Load Balancing (kube-proxy):**
```
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: my-app
  sessionAffinity: ClientIP  # Sticky sessions
  sessionAffinityConfig:
    clientIP:
      timeoutSeconds: 3600
  ports:
  - port: 80
    targetPort: 8080
```

**kube-proxy modes:**
- **iptables** (default): NAT rules, random selection, нет health checks
- **IPVS**: L4 load balancer, multiple algorithms (rr, lc, sh), health checks
- **userspace** (legacy): performance issues

**Ingress Load Balancing:**
```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-ingress
  annotations:
    nginx.ingress.kubernetes.io/load-balance: round_robin
    nginx.ingress.kubernetes.io/upstream-hash-by: "$request_uri"  # Consistent hashing
spec:
  rules:
  - host: example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: my-service
            port:
              number: 80
```

**External LoadBalancer (cloud):**
```
apiVersion: v1
kind: Service
metadata:
  name: my-service
  annotations:
    service.beta.kubernetes.io/aws-load-balancer-type: nlb
    service.beta.kubernetes.io/aws-load-balancer-cross-zone-load-balancing-enabled: "true"
spec:
  type: LoadBalancer
  loadBalancerSourceRanges:
  - 203.0.113.0/24  # Whitelist IPs
  ports:
  - port: 80
```

**Algorithms:** Round Robin (default), Least Connections, IP Hash (session affinity), Weighted, Random.

---

## Вопрос 40

**TLS/SSL в Kubernetes**

TLS/SSL в Kubernetes обеспечивает encrypted communication через certificates хранящиеся в Secrets и managed cert-manager для automatic provisioning/renewal; TLS termination может происходить на Ingress level (decrypt на edge, backend HTTP), Service Mesh level (mTLS между services), или Pod level (end-to-end encryption); cert-manager интегрируется с Let's Encrypt, HashiCorp Vault, self-signed CAs для automated certificate lifecycle.

**TLS Secret:**
```
# Создание TLS secret
kubectl create secret tls tls-secret \
  --cert=path/to/tls.crt \
  --key=path/to/tls.key
```

```
apiVersion: v1
kind: Secret
metadata:
  name: tls-secret
type: kubernetes.io/tls
data:
  tls.crt: <base64-encoded-cert>
  tls.key: <base64-encoded-key>
```

**Ingress TLS:**
```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: tls-ingress
  annotations:
    cert-manager.io/cluster-issuer: letsencrypt-prod
spec:
  tls:
  - hosts:
    - example.com
    - www.example.com
    secretName: tls-secret  # TLS Secret name
  rules:
  - host: example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: my-service
            port:
              number: 80
```

**cert-manager (automated certificates):**
```
# ClusterIssuer для Let's Encrypt
apiVersion: cert-manager.io/v1
kind: ClusterIssuer
metadata:
  name: letsencrypt-prod
spec:
  acme:
    server: https://acme-v02.api.letsencrypt.org/directory
    email: admin@example.com
    privateKeySecretRef:
      name: letsencrypt-prod-key
    solvers:
    - http01:
        ingress:
          class: nginx

# Certificate resource
apiVersion: cert-manager.io/v1
kind: Certificate
metadata:
  name: example-com-tls
spec:
  secretName: example-com-tls
  issuerRef:
    name: letsencrypt-prod
    kind: ClusterIssuer
  dnsNames:
  - example.com
  - www.example.com
```

**mTLS (Service Mesh):**
```
# Istio PeerAuthentication
apiVersion: security.istio.io/v1beta1
kind: PeerAuthentication
metadata:
  name: default
spec:
  mtls:
    mode: STRICT  # Mutual TLS между всеми services
```

**Pod-level TLS:**
```
spec:
  containers:
  - name: app
    volumeMounts:
    - name: tls-certs
      mountPath: /etc/tls
      readOnly: true
  volumes:
  - name: tls-certs
    secret:
      secretName: tls-secret
```

---

## Вопрос 41

**Что такое Volume?**

Volume — это directory доступная containers в Pod для хранения данных, которая outlives container crashes/restarts но не Pod deletion (ephemeral volume) или persistent между Pod recreations (PersistentVolume); поддерживает различные types: emptyDir (temporary scratch space), hostPath (node filesystem), configMap/secret (configuration), PersistentVolumeClaim (persistent storage), и используется для sharing data между containers, сохранения logs, mounting configuration.

**emptyDir (temporary):**
```
spec:
  containers:
  - name: app
    volumeMounts:
    - name: cache
      mountPath: /cache
  volumes:
  - name: cache
    emptyDir: {}  # Удаляется при Pod deletion
    # emptyDir:
    #   medium: Memory  # Использовать RAM (tmpfs)
```

**hostPath (node filesystem):**
```
volumes:
- name: host-data
  hostPath:
    path: /data
    type: DirectoryOrCreate
# Risky: Pod привязан к конкретному node
```

**configMap volume:**
```
volumes:
- name: config
  configMap:
    name: app-config
    items:
    - key: config.yaml
      path: config.yaml
```

**secret volume:**
```
volumes:
- name: secrets
  secret:
    secretName: app-secrets
    defaultMode: 0400  # File permissions
```

**PersistentVolumeClaim:**
```
volumes:
- name: data
  persistentVolumeClaim:
    claimName: my-pvc
```

**Volume types:** emptyDir, hostPath, nfs, iscsi, configMap, secret, persistentVolumeClaim, csi (Container Storage Interface), projected (multiple sources).

---

## Вопрос 42

**PersistentVolume и PersistentVolumeClaim**

PersistentVolume (PV) — это cluster resource представляющий physical storage (disk, NFS, cloud storage) provisioned администратором или dynamically через StorageClass, существующий независимо от Pod lifecycle; PersistentVolumeClaim (PVC) — это request пользователя на storage с specified size/access modes, который Kubernetes binds к доступному PV matching requirements; PV/PVC decouples storage provisioning от consumption и поддерживает reclaim policies (Retain, Delete, Recycle).

**PersistentVolume:**
```
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-data
spec:
  capacity:
    storage: 10Gi
  accessModes:
  - ReadWriteOnce  # RWO, ROX, RWX
  persistentVolumeReclaimPolicy: Retain  # Retain, Delete, Recycle
  storageClassName: manual
  hostPath:  # Или nfs, iscsi, csi, etc
    path: /mnt/data
```

**PersistentVolumeClaim:**
```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-data
spec:
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
      storage: 5Gi
  storageClassName: manual
```

**Использование в Pod:**
```
spec:
  volumes:
  - name: data
    persistentVolumeClaim:
      claimName: pvc-data
  containers:
  - name: app
    volumeMounts:
    - name: data
      mountPath: /data
```

**Access Modes:**
- **ReadWriteOnce (RWO)**: read-write by single node
- **ReadOnlyMany (ROX)**: read-only by multiple nodes
- **ReadWriteMany (RWX)**: read-write by multiple nodes

**Reclaim Policies:**
- **Retain**: manual reclamation (PV остаётся после PVC deletion)
- **Delete**: automatic deletion (PV и underlying storage удаляются)
- **Recycle** (deprecated): basic scrub (`rm -rf /volume/*`)

**Lifecycle:** Available → Bound (PVC binds PV) → Released (PVC deleted) → Reclaim action.

---

## Вопрос 43

**StorageClass**

StorageClass — это Kubernetes resource определяющий "classes" of storage с различными quality-of-service levels, backup policies, и provisioners для dynamic provisioning PersistentVolumes; когда PVC references StorageClass, PV автоматически создаётся on-demand через specified provisioner (AWS EBS, GCE PD, Azure Disk, NFS, Ceph); default StorageClass используется для PVC без explicit storageClassName.

**StorageClass definition:**
```
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: fast-ssd
provisioner: kubernetes.io/aws-ebs  # Cloud provider provisioner
parameters:
  type: gp3
  iopsPerGB: "10"
  encrypted: "true"
reclaimPolicy: Delete
allowVolumeExpansion: true
volumeBindingMode: WaitForFirstConsumer  # или Immediate
```

**Cloud provisioners:**
```
# AWS EBS
provisioner: kubernetes.io/aws-ebs
parameters:
  type: gp3  # gp2, gp3, io1, io2, st1, sc1

# GCE PD
provisioner: kubernetes.io/gce-pd
parameters:
  type: pd-ssd  # pd-standard, pd-ssd

# Azure Disk
provisioner: kubernetes.io/azure-disk
parameters:
  storageaccounttype: Premium_LRS

# NFS (external provisioner)
provisioner: cluster.local/nfs-client-provisioner
```

**PVC с StorageClass:**
```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-fast
spec:
  accessModes:
  - ReadWriteOnce
  storageClassName: fast-ssd  # Reference StorageClass
  resources:
    requests:
      storage: 10Gi
# PV автоматически provisioned
```

**Default StorageClass:**
```
kubectl patch storageclass fast-ssd -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'
```

**volumeBindingMode:**
- **Immediate**: PV создаётся сразу при PVC creation
- **WaitForFirstConsumer**: PV создаётся когда Pod использует PVC (важно для topology constraints)

---

## Вопрос 44

**ConfigMap**

ConfigMap — это Kubernetes API object для хранения non-confidential configuration data в key-value парах, который позволяет decouple configuration от container images делая applications более portable; данные могут быть consumed как environment variables, command-line arguments, или configuration files в volumes; максимальный размер 1MB, для secrets используйте Secret вместо ConfigMap.

**Создание ConfigMap:**
```
# Из literal values
kubectl create configmap app-config \
  --from-literal=database.host=postgres \
  --from-literal=database.port=5432

# Из файла
kubectl create configmap app-config --from-file=config.yaml

# Из директории
kubectl create configmap app-config --from-file=config-dir/
```

**YAML definition:**
```
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  database.host: "postgres"
  database.port: "5432"
  app.properties: |
    server.port=8080
    logging.level=INFO
    feature.enabled=true
```

**Использование как environment variables:**
```
spec:
  containers:
  - name: app
    envFrom:
    - configMapRef:
        name: app-config  # Все keys как env vars
    env:
    - name: DB_HOST  # Specific key
      valueFrom:
        configMapKeyRef:
          name: app-config
          key: database.host
```

**Использование как volume:**
```
spec:
  containers:
  - name: app
    volumeMounts:
    - name: config
      mountPath: /etc/config
      readOnly: true
  volumes:
  - name: config
    configMap:
      name: app-config
      items:  # Optional: select specific keys
      - key: app.properties
        path: application.properties
```

**Immutable ConfigMap:**
```
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
immutable: true  # Нельзя изменить, только recreate
data:
  key: value
```

**Best practices:** Используйте для non-sensitive data, версионируйте через names (app-config-v1), не храните secrets, consider immutable для performance, автоматизируйте updates (GitOps).

---

## Вопрос 45

**Secret**

Secret — это Kubernetes object для хранения sensitive information (passwords, tokens, SSH keys, TLS certificates) в base64-encoded format, предоставляющий более безопасное хранение чем plaintext в ConfigMap через access control (RBAC), encryption at rest (etcd encryption), и automatic decoding при consumption; используется как environment variables, volumes, или imagePullSecrets для registry authentication.

**Типы Secret:**
- **Opaque** (default): arbitrary user-defined data
- **kubernetes.io/tls**: TLS certificate и private key
- **kubernetes.io/dockerconfigjson**: Docker registry credentials
- **kubernetes.io/basic-auth**: username/password
- **kubernetes.io/ssh-auth**: SSH private key
- **kubernetes.io/service-account-token**: ServiceAccount token

**Создание:**
```
# Generic secret
kubectl create secret generic db-secret \
  --from-literal=username=admin \
  --from-literal=password=secret123

# TLS secret
kubectl create secret tls tls-secret \
  --cert=tls.crt \
  --key=tls.key

# Docker registry
kubectl create secret docker-registry regcred \
  --docker-server=registry.example.com \
  --docker-username=user \
  --docker-password=pass
```

**YAML definition:**
```
apiVersion: v1
kind: Secret
metadata:
  name: db-secret
type: Opaque
data:
  username: YWRtaW4=  # base64 encoded "admin"
  password: c2VjcmV0MTIz  # base64 encoded "secret123"
# или
stringData:
  username: admin  # Plain text (auto base64 encoded)
  password: secret123
```

**Использование как env vars:**
```
spec:
  containers:
  - name: app
    env:
    - name: DB_USERNAME
      valueFrom:
        secretKeyRef:
          name: db-secret
          key: username
    - name: DB_PASSWORD
      valueFrom:
        secretKeyRef:
          name: db-secret
          key: password
```

**Использование как volume:**
```
spec:
  containers:
  - name: app
    volumeMounts:
    - name: secrets
      mountPath: /etc/secrets
      readOnly: true
  volumes:
  - name: secrets
    secret:
      secretName: db-secret
      defaultMode: 0400  # Permissions
```

**Encryption at rest:**
```
# EncryptionConfiguration для etcd
apiVersion: apiserver.config.k8s.io/v1
kind: EncryptionConfiguration
resources:
- resources:
  - secrets
  providers:
  - aescbc:
      keys:
      - name: key1
        secret: <base64-encoded-32-byte-key>
  - identity: {}
```

**Best practices:** Используйте RBAC для ограничения access, enable encryption at rest, consider external secret management (Vault, AWS Secrets Manager), не commit secrets в Git, регулярно rotate secrets.

---

## Вопрос 46

**RBAC (Role-Based Access Control)**

RBAC — это метод регулирования доступа к Kubernetes API resources на основе ролей пользователей, использующий четыре API objects: Role (permissions в namespace), ClusterRole (cluster-wide permissions), RoleBinding (bind Role к subjects в namespace), ClusterRoleBinding (bind ClusterRole cluster-wide); определяет WHO (users, groups, service accounts) может делать WHAT (verbs: get, list, create, delete) с WHICH resources (pods, services, secrets).

**Role (namespace-scoped):**
```
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: pod-reader
  namespace: default
rules:
- apiGroups: [""]  # "" = core API group
  resources: ["pods"]
  verbs: ["get", "list", "watch"]
- apiGroups: ["apps"]
  resources: ["deployments"]
  verbs: ["get", "list"]
```

**ClusterRole (cluster-wide):**
```
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: cluster-admin
rules:
- apiGroups: ["*"]
  resources: ["*"]
  verbs: ["*"]
```

**RoleBinding:**
```
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: read-pods
  namespace: default
subjects:
- kind: User
  name: jane
  apiGroup: rbac.authorization.k8s.io
- kind: ServiceAccount
  name: my-service-account
  namespace: default
roleRef:
  kind: Role
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io
```

**ClusterRoleBinding:**
```
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: cluster-admin-binding
subjects:
- kind: User
  name: admin
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: ClusterRole
  name: cluster-admin
  apiGroup: rbac.authorization.k8s.io
```

**ServiceAccount:**
```
apiVersion: v1
kind: ServiceAccount
metadata:
  name: my-service-account
  namespace: default
***
# Pod использует ServiceAccount
spec:
  serviceAccountName: my-service-account
```

**Verbs:** get, list, watch, create, update, patch, delete, deletecollection.

**Проверка permissions:**
```
# Проверить может ли user
kubectl auth can-i create deployments --as=jane

# Проверить для service account
kubectl auth can-i list pods --as=system:serviceaccount:default:my-sa

# Who can
kubectl auth can-i --list --as=jane
```

**Best practices:** Principle of least privilege, используйте ServiceAccounts для pods, avoid cluster-admin где возможно, namespace-level roles где применимо, регулярный audit permissions.

---

## Вопрос 47

**Security Context**

Security Context — это pod/container-level настройки определяющие privilege и access control settings: runAsUser (UID процесса), runAsGroup (GID), fsGroup (volume ownership), capabilities (Linux capabilities), readOnlyRootFilesystem, allowPrivilegeEscalation, SELinux labels; применяется для least privilege principle, container hardening, и compliance требований, где container-level settings override pod-level.

**Pod-level Security Context:**
```
spec:
  securityContext:
    runAsUser: 1000        # UID
    runAsGroup: 3000       # GID
    fsGroup: 2000          # Volume ownership GID
    runAsNonRoot: true     # Enforce non-root
    seccompProfile:
      type: RuntimeDefault
```

**Container-level Security Context:**
```
spec:
  containers:
  - name: app
    securityContext:
      runAsUser: 2000
      runAsNonRoot: true
      allowPrivilegeEscalation: false
      readOnlyRootFilesystem: true
      capabilities:
        drop:
        - ALL
        add:
        - NET_BIND_SERVICE  # Bind ports < 1024
```

**Privileged container:**
```
securityContext:
  privileged: true  # Full host access (избегайте!)
```

**SELinux:**
```
securityContext:
  seLinuxOptions:
    level: "s0:c123,c456"
```

**Seccomp profile:**
```
securityContext:
  seccompProfile:
    type: RuntimeDefault  # или Localhost
    localhostProfile: profiles/audit.json
```

**Best practices:**
```
# Recommended security baseline
securityContext:
  runAsNonRoot: true
  runAsUser: 10000
  allowPrivilegeEscalation: false
  readOnlyRootFilesystem: true
  capabilities:
    drop:
    - ALL
  seccompProfile:
    type: RuntimeDefault
```

**Pod Security Standards:** Privileged (unrestricted), Baseline (minimally restrictive), Restricted (heavily restricted, security best practices).

---

## Вопрос 48

**Мониторинг и логирование**

Мониторинг в Kubernetes собирает metrics (CPU, memory, network) через Metrics Server (resource metrics) и Prometheus (custom metrics) для observability, alerting, и autoscaling; логирование агрегирует container logs через stdout/stderr в centralized system (ELK Stack, Loki, Fluentd) для debugging и audit; distributed tracing (Jaeger, Zipkin) tracks requests через microservices для performance analysis.

**Metrics Server (resource metrics):**
```
# Установка
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml

# Использование
kubectl top nodes
kubectl top pods
kubectl top pods --containers
```

**Prometheus Stack:**
```
# Prometheus Operator + Grafana
# ServiceMonitor для scraping metrics
apiVersion: monitoring.coreos.com/v1
kind: ServiceMonitor
metadata:
  name: app-metrics
spec:
  selector:
    matchLabels:
      app: my-app
  endpoints:
  - port: metrics
    interval: 30s
```

**Application metrics (Prometheus format):**
```
spec:
  containers:
  - name: app
    ports:
    - name: metrics
      containerPort: 8080
    # App exposes /metrics endpoint
```

**Logging (EFK Stack):**
```
# Fluentd DaemonSet собирает logs
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: fluentd
spec:
  template:
    spec:
      containers:
      - name: fluentd
        image: fluent/fluentd-kubernetes-daemonset:v1-debian-elasticsearch
        env:
        - name: FLUENT_ELASTICSEARCH_HOST
          value: "elasticsearch.logging.svc.cluster.local"
        volumeMounts:
        - name: varlog
          mountPath: /var/log
        - name: varlibdockercontainers
          mountPath: /var/lib/docker/containers
          readOnly: true
```

**Structured logging:**
```
{"timestamp":"2023-10-20T10:30:00Z","level":"INFO","message":"Request processed","request_id":"abc123"}
```

**Viewing logs:**
```
kubectl logs pod-name
kubectl logs pod-name -c container-name
kubectl logs -f pod-name  # Follow
kubectl logs pod-name --previous  # Previous container
kubectl logs -l app=my-app  # All pods with label
```

**Tools:**
- **Metrics**: Metrics Server, Prometheus, Datadog, New Relic
- **Logging**: EFK (Elasticsearch/Fluentd/Kibana), Loki/Grafana, Splunk
- **Tracing**: Jaeger, Zipkin, AWS X-Ray
- **APM**: Datadog, New Relic, Dynatrace

---

## Вопрос 49

**Операторы Kubernetes**

Operator — это pattern расширяющий Kubernetes API через Custom Resource Definitions (CRDs) и custom controllers для автоматизации управления complex stateful applications (databases, message queues, monitoring), кодируя domain-specific operational knowledge (backup, restore, upgrade, scaling) и следуя reconciliation loop principle; Operator Framework (Operator SDK) упрощает разработку operators с Go/Ansible/Helm-based approaches.

**Архитектура Operator:**
```
Custom Resource (CR)
      ↓
  CRD Definition
      ↓
Operator Controller (watches CRs)
      ↓
Reconciliation Logic
      ↓
Kubernetes API (creates/updates resources)
```

**Custom Resource Definition:**
```
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
  name: databases.example.com
spec:
  group: example.com
  names:
    kind: Database
    plural: databases
    singular: database
  scope: Namespaced
  versions:
  - name: v1
    served: true
    storage: true
    schema:
      openAPIV3Schema:
        type: object
        properties:
          spec:
            type: object
            properties:
              replicas:
                type: integer
              version:
                type: string
```

**Custom Resource:**
```
apiVersion: example.com/v1
kind: Database
metadata:
  name: my-database
spec:
  replicas: 3
  version: "13.5"
  backup:
    enabled: true
    schedule: "0 2 * * *"
```

**Популярные Operators:**
- **Prometheus Operator**: управление Prometheus, Alertmanager, ServiceMonitor
- **PostgreSQL Operator**: Zalando/Crunchy, HA PostgreSQL clusters
- **MongoDB Operator**: MongoDB Enterprise Kubernetes
- **Elasticsearch Operator**: ECK (Elastic Cloud on Kubernetes)
- **Kafka Operator**: Strimzi
- **MySQL Operator**: Oracle MySQL Operator
- **Redis Operator**: spotahome redis-operator

**Operator Lifecycle:**
1. Install Operator (Deployment + RBAC)
2. Create CR (desired state)
3. Operator watches CR
4. Reconciles actual state → desired state
5. Continuous monitoring и adjustments

**Use cases:** Database management (backup, restore, upgrades), Certificate management (cert-manager), Service mesh (Istio), Monitoring (Prometheus), CI/CD (ArgoCD, Tekton).

---

## Вопрос 50

**Best Practices для Production**

Production Kubernetes требует: resource management (requests/limits для всех containers), health checks (liveness/readiness probes), security (RBAC, NetworkPolicy, Pod Security Standards, encrypted secrets), high availability (multiple replicas, PodDisruptionBudget, anti-affinity), observability (metrics, logging, tracing), GitOps для deployments, automated backups, disaster recovery plan, regular updates, и cost optimization через resource right-sizing.

**Resource Management:**
```
resources:
  requests:  # Всегда устанавливайте
    memory: "256Mi"
    cpu: "250m"
  limits:
    memory: "512Mi"
    cpu: "500m"
```

**Health Checks:**
```
livenessProbe:
  httpGet:
    path: /healthz
    port: 8080
  initialDelaySeconds: 30
readinessProbe:
  httpGet:
    path: /ready
    port: 8080
  initialDelaySeconds: 5
```

**High Availability:**
```
spec:
  replicas: 3  # Минимум 3 для HA
  affinity:
    podAntiAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
      - labelSelector:
          matchLabels:
            app: my-app
        topologyKey: kubernetes.io/hostname
***
apiVersion: policy/v1
kind: PodDisruptionBudget
metadata:
  name: app-pdb
spec:
  minAvailable: 2
  selector:
    matchLabels:
      app: my-app
```

**Security:**
```
securityContext:
  runAsNonRoot: true
  runAsUser: 10000
  allowPrivilegeEscalation: false
  readOnlyRootFilesystem: true
  capabilities:
    drop: ["ALL"]
---
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: deny-all
spec:
  podSelector: {}
  policyTypes:
  - Ingress
  - Egress
```

**Deployment Strategies:**
- Blue-Green или Canary deployments
- Automated rollback при failures
- Gradual rollout с monitoring

**Backup & DR:**
- Regular etcd backups
- PersistentVolume snapshots
- Multi-region setup для critical services
- Documented disaster recovery procedures

**Cost Optimization:**
- Right-size resources (мониторьте actual usage)
- Use HPA для auto-scaling
- Spot/Preemptible instances для non-critical workloads
- Resource quotas per namespace

**GitOps:**
- Infrastructure as Code (Git as single source of truth)
- ArgoCD/FluxCD для automated deployments
- Pull-based deployment model
- Audit trail через Git history

**Monitoring & Alerting:**
- Prometheus + Grafana dashboards
- Alerts на critical metrics
- Log aggregation (EFK/Loki)
- Distributed tracing

**Updates:**
- Regular Kubernetes version upgrades
- Security patches
- Node OS updates
- Testing на staging перед production

---

## Вопрос 51

**Что такое Helm?**

Helm — это package manager для Kubernetes, который упрощает развертывание и управление приложениями через Charts (пакеты предконфигурированных Kubernetes ресурсов); предоставляет шаблонизацию YAML манифестов с переменными, versioning releases, rollback capability, и централизованное управление конфигурацией через values.yaml.

**Основные концепции:**
- **Chart**: пакет с templates и конфигурацией
- **Release**: установленный instance чарта в кластере
- **Repository**: хранилище charts
- **Values**: конфигурационные параметры

**Базовые команды:**
```
helm install my-release bitnami/nginx
helm upgrade my-release bitnami/nginx --set replicaCount=3
helm rollback my-release 1
helm uninstall my-release
helm list
helm history my-release
```

---

## Вопрос 52

**Что такое Helm Chart и структура?**

Helm Chart — это набор файлов, описывающих Kubernetes ресурсы для развертывания приложения, организованный в стандартизированную структуру с обязательными Chart.yaml (метаданные) и templates/ (Kubernetes манифесты с Go templating), а также values.yaml (default конфигурация), charts/ (зависимости).

**Структура:**
```
my-chart/
├── Chart.yaml
├── values.yaml
├── charts/
├── templates/
│   ├── deployment.yaml
│   ├── service.yaml
│   ├── _helpers.tpl
│   └── NOTES.txt
└── .helmignore
```

**Chart.yaml:**
```
apiVersion: v2
name: my-app
version: 1.0.0
appVersion: "2.1.0"
dependencies:
  - name: postgresql
    version: "12.x.x"
    repository: https://charts.bitnami.com/bitnami
```

---

## Вопрос 53

**Как установить/обновить/удалить Helm chart?**

Установка, обновление и удаление выполняются через команды install, upgrade и uninstall, где install создаёт новый release, upgrade применяет изменения с сохранением истории для rollback, uninstall удаляет все ресурсы release.

**Установка:**
```
helm install my-release bitnami/nginx
helm install my-release bitnami/nginx -f values.yaml
helm install my-release bitnami/nginx --set replicaCount=3
helm install my-release ./my-chart/ --dry-run --debug
helm install my-release bitnami/nginx --wait --timeout 5m
```

**Обновление:**
```
helm upgrade my-release bitnami/nginx
helm upgrade my-release bitnami/nginx -f values-prod.yaml
helm upgrade my-release bitnami/nginx --install  # Install если не существует
helm upgrade my-release bitnami/nginx --atomic   # Rollback при failure
```

**Откат:**
```
helm history my-release
helm rollback my-release
helm rollback my-release 3
```

**Удаление:**
```
helm uninstall my-release
helm uninstall my-release --keep-history
```

---

## Вопрос 54

**Что такое values.yaml и переопределение значений?**

values.yaml — файл с дефолтными параметрами конфигурации чарта, из которого формируется объект .Values используемый шаблонами; значения могут быть переопределены через -f files и --set параметры в порядке возрастания специфичности.

**Приоритет:**
```
1. values.yaml (default)
2. -f values-file1.yaml
3. -f values-file2.yaml
4. --set key=value (highest)
```

**values.yaml:**
```
replicaCount: 2
image:
  repository: nginx
  tag: "1.21.0"
resources:
  limits:
    cpu: 100m
    memory: 128Mi
```

**Переопределение:**
```
helm install my-release ./chart --set replicaCount=5
helm install my-release ./chart -f values-prod.yaml
helm install my-release ./chart -f common.yaml -f prod.yaml --set image.tag=1.22.0
```

---

## Вопрос 55

**Helm Templates и Go templating**

Helm Templates используют Go template engine для генерации Kubernetes манифестов с динамической подстановкой значений из .Values, .Release, .Chart; поддерживают условную логику, циклы, функции Sprig library, include/define для reusable blocks.

**Встроенные объекты:**
```
{{ .Values.replicaCount }}
{{ .Release.Name }}
{{ .Release.Namespace }}
{{ .Chart.Name }}
{{ .Chart.Version }}
```

**Условная логика:**
```
{{- if .Values.ingress.enabled }}
apiVersion: networking.k8s.io/v1
kind: Ingress
{{- end }}
```

**Циклы:**
```
{{- range .Values.hosts }}
- host: {{ . }}
{{- end }}
```

**Функции:**
```
{{ .Values.name | upper }}
{{ .Values.port | default 8080 }}
{{ .Values.name | quote }}
{{ uuidv4 }}
```

**Named templates:**
```
{{- define "mychart.labels" -}}
app: {{ .Chart.Name }}
{{- end }}

metadata:
  labels:
    {{- include "mychart.labels" . | nindent 4 }}
```

---

## Вопрос 56

**Что такое Helm Hooks?**

Helm Hooks — механизм lifecycle management для выполнения специфических ресурсов (Jobs/Pods) в определённые моменты: pre-install, post-install, pre-upgrade, post-upgrade, pre-delete, post-delete, test; определяются через аннотацию helm.sh/hook.

**Hook манифест:**
```
apiVersion: batch/v1
kind: Job
metadata:
  name: {{ .Release.Name }}-post-install
  annotations:
    helm.sh/hook: post-install
    helm.sh/hook-weight: "5"
    helm.sh/hook-delete-policy: hook-succeeded
spec:
  template:
    spec:
      restartPolicy: Never
      containers:
      - name: post-install
        image: busybox
        command: ['sh', '-c', 'echo Done']
```

**Типы hooks:** pre-install, post-install, pre-delete, post-delete, pre-upgrade, post-upgrade, pre-rollback, post-rollback, test.

**Delete policies:** before-hook-creation, hook-succeeded, hook-failed.

---

## Вопрос 57

**Как управлять зависимостями в charts?**

Зависимости управляются через раздел dependencies в Chart.yaml, где каждая зависимость определяется именем, версией, repository URL; загружаются командой helm dependency update в charts/ директорию.

**Chart.yaml:**
```
dependencies:
  - name: postgresql
    version: "12.1.5"
    repository: https://charts.bitnami.com/bitnami
    condition: postgresql.enabled
  - name: redis
    version: "17.x.x"
    repository: https://charts.bitnami.com/bitnami
    alias: cache
```

**Команды:**
```
helm dependency update ./my-chart
helm dependency build ./my-chart
helm dependency list ./my-chart
```

**Конфигурация subcharts:**
```
global:
  storageClass: "fast-ssd"

postgresql:
  enabled: true
  auth:
    username: myuser
    password: mypass
```

---

## Вопрос 58

**Helm Repository — работа с репозиториями**

Helm Repository — это HTTP server хранящий indexed charts, позволяющий sharing и distribution чартов; поддерживает public repos (Bitnami, stable), private repos (Harbor, ChartMuseum, Artifactory), OCI registries.

**Работа с repos:**
```
# Добавить repo
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo add stable https://charts.helm.sh/stable

# Обновить index
helm repo update

# Поиск charts
helm search repo nginx
helm search hub wordpress  # Artifact Hub

# Список repos
helm repo list

# Удалить repo
helm repo remove bitnami
```

**Создание собственного repo:**
```
# Package charts
helm package my-chart/

# Generate index
helm repo index . --url https://my-charts.example.com

# Upload index.yaml и .tgz файлы на HTTP server
```

**OCI Registry (Helm 3.8+):**
```
# Login
echo $PASSWORD | helm registry login registry.example.com -u $USERNAME --password-stdin

# Push chart
helm push my-chart-1.0.0.tgz oci://registry.example.com/charts

# Install from OCI
helm install my-release oci://registry.example.com/charts/my-chart --version 1.0.0
```

---

## Вопрос 59

**Как тестировать и отлаживать charts?**

Тестирование charts включает lint (syntax check), dry-run (template rendering без apply), template debugging, automated tests через helm test, и integration testing в real cluster.

**Lint:**
```
helm lint my-chart/
# Проверяет syntax errors, best practices
```

**Dry-run и template:**
```
# Render templates без установки
helm install my-release my-chart/ --dry-run --debug

# Template только (no cluster connection)
helm template my-release my-chart/
helm template my-release my-chart/ -f values-prod.yaml
helm template my-release my-chart/ --show-only templates/deployment.yaml
```

**Helm test:**
```
# templates/tests/test-connection.yaml
apiVersion: v1
kind: Pod
metadata:
  name: {{ .Release.Name }}-test
  annotations:
    helm.sh/hook: test
spec:
  containers:
  - name: wget
    image: busybox
    command: ['wget']
    args: ['{{ .Release.Name }}-service:80']
  restartPolicy: Never
```

```
# Запуск тестов
helm test my-release
```

**Debugging:**
```
# Подробный output
helm install my-release my-chart/ --debug

# Проверка values
helm get values my-release
helm get values my-release --all

# Проверка manifest
helm get manifest my-release

# Diff перед upgrade
helm diff upgrade my-release my-chart/ -f new-values.yaml
```

**Best practices:** Всегда lint перед package, dry-run перед production install, version control charts в Git, automated testing в CI/CD, staging environment для validation.

---

## Вопрос 60

**Что такое Helmfile и зачем он нужен?**

Helmfile — это declarative spec для deploying multiple Helm charts в reproducible manner, определяющий набор releases с их values, dependencies, environments через единый helmfile.yaml; упрощает управление complex deployments с multiple charts, синхронизирует desired state с cluster, поддерживает environments (dev/staging/prod) и secrets encryption.

**helmfile.yaml:**
```
repositories:
  - name: bitnami
    url: https://charts.bitnami.com/bitnami

releases:
  - name: postgresql
    namespace: database
    chart: bitnami/postgresql
    version: 12.1.5
    values:
      - ./values/postgresql.yaml
    set:
      - name: auth.database
        value: mydb

  - name: redis
    namespace: cache
    chart: bitnami/redis
    version: 17.3.7
    values:
      - ./values/redis.yaml

  - name: my-app
    namespace: default
    chart: ./charts/my-app
    needs:
      - database/postgresql
      - cache/redis
    values:
      - ./values/my-app-{{ .Environment.Name }}.yaml
```

**Environments:**
```
environments:
  development:
    values:
      - environments/dev/values.yaml
  production:
    values:
      - environments/prod/values.yaml
```

**Команды:**
```
# Apply all releases
helmfile apply

# Sync environment
helmfile -e production sync

# Diff before apply
helmfile diff

# Template (render без apply)
helmfile template

# List releases
helmfile list

# Destroy all releases
helmfile destroy
```

**Secrets management:**
```
releases:
  - name: my-app
    values:
      - secrets://values/secrets.yaml  # helm-secrets plugin
```

**Use cases:** Multi-chart deployments, Environment management (dev/staging/prod), GitOps workflows, Team collaboration, Reproducible deployments.

---

## Вопрос 61

**Различия Helm 2 и Helm 3**

Helm 3 — major rewrite с breaking changes: удалён Tiller (server-side component, улучшена security), release storage в Secrets (вместо ConfigMaps), three-way strategic merge patches, улучшенная upgrade strategy, JSON Schema validation, library charts, OCI registry support, namespace required при install.

**Основные отличия:**

**Tiller removal:**
```
Helm 2: Client → Tiller (server) → Kubernetes API
Helm 3: Client → Kubernetes API (direct)

Преимущества:
✅ Улучшенная security (no elevated privileges)
✅ Проще installation (no Tiller deploy)
✅ RBAC работает directly
```

**Release storage:**
```
Helm 2: ConfigMaps в kube-system namespace
Helm 3: Secrets в release namespace

Helm 3:
- Лучше security (Secrets)
- Release info в том же namespace
```

**Three-way merge:**
```
Helm 2: Two-way merge (old manifest + new manifest)
Helm 3: Three-way merge (old + new + live state)

Преимущества:
- Обнаружение manual changes
- Smarter conflict resolution
```

**Другие изменения:**
- **Namespace required**: `helm install my-release chart/ -n namespace` (обязательно)
- **Chart.yaml v2**: новые поля (type, dependencies)
- **JSON Schema**: validation для values.yaml
- **Library charts**: type: library для reusable templates
- **OCI support**: push/pull charts в container registries
- **Remove/deprecate**: helm serve, helm reset

**Migration Helm 2 → 3:**
```
# Helm 2to3 plugin
helm plugin install https://github.com/helm/helm-2to3

# Migrate config
helm 2to3 move config

# Migrate releases
helm 2to3 convert RELEASE_NAME

# Cleanup Helm 2
helm 2to3 cleanup
```

---

## Вопрос 62

**Управление секретами в Helm**

Управление секретами в Helm обеспечивается через: helm-secrets plugin (encrypts values files с SOPS), sealed-secrets (encrypts Secrets в Git-safe format), external secret managers (Vault, AWS Secrets Manager integration), и Secret resources в charts с encrypted values; предотвращает commit plain-text secrets в Git и обеспечивает secure secret distribution.

**helm-secrets plugin:**
```
# Установка
helm plugin install https://github.com/jkroepke/helm-secrets

# Encrypt values file
helm secrets encrypt secrets.yaml

# Creates secrets.yaml.dec (encrypted)
# Edit encrypted file
helm secrets edit secrets.yaml

# Decrypt (temporary, for viewing)
helm secrets view secrets.yaml

# Install с encrypted values
helm secrets install my-release my-chart/ -f secrets.yaml
```

**SOPS config (.sops.yaml):**
```
creation_rules:
  - path_regex: secrets.*\.yaml
    encrypted_regex: ^(data|stringData)$
    kms: arn:aws:kms:us-west-2:123456789:key/abcd-1234
    # или
    pgp: 3A2C4F5E6B7D8A9C
```

**Sealed Secrets:**
```
# Установка controller
kubectl apply -f https://github.com/bitnami-labs/sealed-secrets/releases/download/v0.18.0/controller.yaml

# Encrypt secret
kubeseal --format yaml < secret.yaml > sealed-secret.yaml

# Commit sealed-secret.yaml в Git (safe)
# Controller auto-decrypts в cluster
```

**External Secrets Operator:**
```
apiVersion: external-secrets.io/v1beta1
kind: ExternalSecret
metadata:
  name: db-secret
spec:
  refreshInterval: 1h
  secretStoreRef:
    name: vault-backend
    kind: SecretStore
  target:
    name: db-secret
  data:
  - secretKey: password
    remoteRef:
      key: database/password
```

**Vault integration:**
```
# Vault Agent injector
annotations:
  vault.hashicorp.com/agent-inject: "true"
  vault.hashicorp.com/role: "myapp"
  vault.hashicorp.com/agent-inject-secret-config: "secret/data/config"
```

**Best practices:**
- Никогда не commit plain-text secrets
- Используйте encryption (SOPS, Sealed Secrets)
- Consider external secret managers для production
- Rotate secrets регулярно
- Limit secret access через RBAC
- Audit secret usage

---

## Вопрос 63

**Helm в CI/CD**

Helm в CI/CD автоматизирует deployments через pipelines интегрируясь с GitOps (ArgoCD, FluxCD), GitHub Actions, GitLab CI, Jenkins; обеспечивает versioned releases, rollback capability, environment-specific values, automated testing, и continuous delivery в Kubernetes clusters.

**GitHub Actions:**
```
name: Deploy to Kubernetes
on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3
    
    - name: Configure kubectl
      uses: azure/k8s-set-context@v3
      with:
        kubeconfig: ${{ secrets.KUBE_CONFIG }}
    
    - name: Install Helm
      uses: azure/setup-helm@v3
      with:
        version: '3.12.0'
    
    - name: Lint chart
      run: helm lint ./chart
    
    - name: Deploy
      run: |
        helm upgrade my-app ./chart \
          --install \
          --namespace production \
          --create-namespace \
          --values ./values/production.yaml \
          --set image.tag=${{ github.sha }} \
          --wait \
          --timeout 5m
```

**GitLab CI:**
```
stages:
  - test
  - deploy

lint:
  stage: test
  script:
    - helm lint ./chart

deploy:production:
  stage: deploy
  script:
    - helm upgrade my-app ./chart
        --install
        --namespace production
        --values values/production.yaml
        --set image.tag=$CI_COMMIT_SHA
        --atomic
  only:
    - main
  environment:
    name: production
    kubernetes:
      namespace: production
```

**ArgoCD (GitOps):**
```
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: my-app
  namespace: argocd
spec:
  project: default
  source:
    repoURL: https://github.com/company/charts
    targetRevision: HEAD
    path: my-app
    helm:
      valueFiles:
      - values-production.yaml
      parameters:
      - name: image.tag
        value: v1.2.3
  destination:
    server: https://kubernetes.default.svc
    namespace: production
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
```

**Best practices:**
- Версионируйте charts в Git
- Automated testing (lint, dry-run)
- Environment-specific values files
- Image tag через CI/CD variables
- Rollback strategy при failures
- Notifications (Slack, email)
- GitOps для production (ArgoCD/Flux)

---

## Вопрос 64

**Chart Hooks vs. Kubernetes Jobs**

Chart Hooks — это Helm-specific механизм lifecycle management выполняющий resources в defined moments release lifecycle (pre/post install/upgrade/delete), управляемый Helm и влияющий на release status; Kubernetes Jobs — это native K8s resources для batch/one-time tasks независимые от Helm, управляемые Job controller и не blocking release operations.

**Сравнение:**

**Chart Hooks:**
```
apiVersion: batch/v1
kind: Job
metadata:
  annotations:
    helm.sh/hook: pre-upgrade
    helm.sh/hook-weight: "5"
    helm.sh/hook-delete-policy: hook-succeeded
spec:
  template:
    spec:
      containers:
      - name: migration
        image: migrate:latest
      restartPolicy: Never
```

**Characteristics:**
- Triggered by Helm operations
- Blocks release until completion
- Managed by Helm (tracking, cleanup)
- Влияет на release success/failure
- Порядок через hook-weight
- Delete policies

**Kubernetes Jobs:**
```
apiVersion: batch/v1
kind: Job
metadata:
  name: data-migration
spec:
  completions: 1
  parallelism: 1
  template:
    spec:
      containers:
      - name: migration
        image: migrate:latest
      restartPolicy: OnFailure
```

**Characteristics:**
- Independent resource
- Non-blocking (async)
- Managed by Job controller
- Не влияет на Helm release
- Can run любое время
- Manual cleanup

**Когда использовать:**

**Chart Hooks:**
- Database migrations перед upgrade
- Pre-install validation
- Post-deploy notifications
- Cleanup tasks перед delete
- Tasks тесно связанные с release lifecycle

**Kubernetes Jobs:**
- Regular batch processing
- Scheduled tasks (с CronJob)
- Independent data processing
- Tasks не связанные с deployments
- Long-running batch jobs

**CronJob (scheduled Jobs):**
```
apiVersion: batch/v1
kind: CronJob
metadata:
  name: backup
spec:
  schedule: "0 2 * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: backup
            image: backup-tool:latest
          restartPolicy: OnFailure
```

---

## Вопрос 65

**Library Chart — как создать?**

Library Chart — это Helm chart с type: library содержащий reusable templates и helper functions без deployable resources, используемый другими charts через dependencies для sharing common patterns (labels, annotations, resource definitions); не может быть installed напрямую, только imported как dependency.

**Создание Library Chart:**

**Chart.yaml:**
```
apiVersion: v2
name: common-lib
description: Common library chart with reusable templates
type: library  # Тип library (не application)
version: 1.0.0
```

**templates/_helpers.tpl:**
```
{{/*
Standard labels
*/}}
{{- define "common-lib.labels" -}}
helm.sh/chart: {{ .Chart.Name }}-{{ .Chart.Version }}
app.kubernetes.io/name: {{ .Chart.Name }}
app.kubernetes.io/instance: {{ .Release.Name }}
app.kubernetes.io/version: {{ .Chart.AppVersion | quote }}
app.kubernetes.io/managed-by: {{ .Release.Service }}
{{- end }}

{{/*
Selector labels
*/}}
{{- define "common-lib.selectorLabels" -}}
app.kubernetes.io/name: {{ .Chart.Name }}
app.kubernetes.io/instance: {{ .Release.Name }}
{{- end }}

{{/*
Create a default fully qualified app name
*/}}
{{- define "common-lib.fullname" -}}
{{- printf "%s-%s" .Release.Name .Chart.Name | trunc 63 | trimSuffix "-" -}}
{{- end }}

{{/*
Common annotations
*/}}
{{- define "common-lib.annotations" -}}
meta.helm.sh/release-name: {{ .Release.Name }}
meta.helm.sh/release-namespace: {{ .Release.Namespace }}
{{- end }}

{{/*
Security context
*/}}
{{- define "common-lib.securityContext" -}}
runAsNonRoot: true
runAsUser: 10000
allowPrivilegeEscalation: false
readOnlyRootFilesystem: true
capabilities:
  drop:
  - ALL
{{- end }}
```

**Использование Library Chart:**

**Parent Chart Chart.yaml:**
```
apiVersion: v2
name: my-app
version: 1.0.0
dependencies:
  - name: common-lib
    version: 1.0.0
    repository: https://charts.example.com
    # или
    repository: file://../common-lib
```

**Parent Chart templates/deployment.yaml:**
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ include "common-lib.fullname" . }}
  labels:
    {{- include "common-lib.labels" . | nindent 4 }}
  annotations:
    {{- include "common-lib.annotations" . | nindent 4 }}
spec:
  selector:
    matchLabels:
      {{- include "common-lib.selectorLabels" . | nindent 6 }}
  template:
    metadata:
      labels:
        {{- include "common-lib.labels" . | nindent 8 }}
    spec:
      securityContext:
        {{- include "common-lib.securityContext" . | nindent 8 }}
      containers:
      - name: app
        image: my-app:latest
```

**Best practices:**
- Документируйте все defined templates
- Версионируйте library charts
- Тестируйте с multiple parent charts
- Keep templates generic и configurable
- Namespace template names (chart-name.template-name)
- Regular updates и maintenance

**Use cases:**
- Common labels/annotations patterns
- Standard security contexts
- Resource definitions (CPU/memory templates)
- Probe configurations
- Shared helper functions
- Organization-wide standards
