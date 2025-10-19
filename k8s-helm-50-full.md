# 50 вопросов и ответов для собеседования по Kubernetes и Helm Charts

## Содержание

### Kubernetes (вопросы 1–20)

1. [Что такое Kubernetes?](#вопрос-1)
2. [Какие основные компоненты кластера Kubernetes?](#вопрос-2)
3. [Что такое Pod в Kubernetes?](#вопрос-3)
4. [В чем разница между Deployment и StatefulSet?](#вопрос-4)
5. [Что такое Service и какие типы существуют?](#вопрос-5)
6. [Что такое Namespace?](#вопрос-6)
7. [Что такое ConfigMap и Secret? Отличия?](#вопрос-7)
8. [Что такое Ingress?](#вопрос-8)
9. [Что такое DaemonSet? Когда использовать?](#вопрос-9)
10. [Что такое PersistentVolume и PersistentVolumeClaim?](#вопрос-10)
11. [Что такое Liveness, Readiness и Startup пробы?](#вопрос-11)
12. [Что такое RBAC в Kubernetes?](#вопрос-12)
13. [Что такое HorizontalPodAutoscaler (HPA)?](#вопрос-13)
14. [Что такое ResourceQuota и LimitRange?](#вопрос-14)
15. [Что такое Rolling Update и как откатить?](#вопрос-15)
16. [Что такое NetworkPolicy?](#вопрос-16)
17. [Jobs и CronJobs — как работают?](#вопрос-17)
18. [Что такое InitContainers?](#вопрос-18)
19. [Что такое Affinity и Anti-Affinity?](#вопрос-19)
20. [Что такое Taints и Tolerations?](#вопрос-20)

### Helm Charts (вопросы 21–35)

21. [Что такое Helm?](#вопрос-21)
22. [Что такое Helm Chart и структура?](#вопрос-22)
23. [Как установить/обновить/удалить Helm chart?](#вопрос-23)
24. [Что такое values.yaml и переопределение значений?](#вопрос-24)
25. [Helm Templates и Go templating](#вопрос-25)
26. [Что такое Helm Hooks?](#вопрос-26)
27. [Как управлять зависимостями в charts?](#вопрос-27)
28. [Helm Repository — работа с репозиториями](#вопрос-28)
29. [Как тестировать и отлаживать charts?](#вопрос-29)
30. [Что такое Helmfile и зачем он нужен?](#вопрос-30)
31. [Различия Helm 2 и Helm 3](#вопрос-31)
32. [Управление секретами в Helm](#вопрос-32)
33. [Helm в CI/CD](#вопрос-33)
34. [Chart Hooks vs. Kubernetes Jobs](#вопрос-34)
35. [Library Chart — как создать?](#вопрос-35)

### Продвинутые темы (вопросы 36–50)

36. [ChartMuseum — как использовать?](#вопрос-36)
37. [Kubernetes DNS и Service Discovery](#вопрос-37)
38. [Operators и CRD](#вопрос-38)
39. [Мониторинг Prometheus + Grafana](#вопрос-39)
40. [Blue-Green и Canary деплойменты](#вопрос-40)
41. [Scheduler и влияние на планирование](#вопрос-41)
42. [Kubernetes Volumes — типы](#вопрос-42)
43. [Безопасность контейнеров](#вопрос-43)
44. [Admission Controllers](#вопрос-44)
45. [Garbage Collection](#вопрос-45)
46. [Custom Resource Definitions (CRD)](#вопрос-46)
47. [etcd — как работает и зачем?](#вопрос-47)
48. [Resource Requests и Limits](#вопрос-48)
49. [Service Mesh и Istio](#вопрос-49)
50. [Multi-tenancy в Kubernetes](#вопрос-50)

***

## Вопрос 1
### Что такое Kubernetes?
Ответ: Kubernetes — платформа для оркестрации контейнерных приложений, автоматизирующая деплой, масштабирование и управление в любых средах, обеспечивая высокую доступность и отказоустойчивость.

## Вопрос 2
### Какие основные компоненты кластера Kubernetes?
Ответ:
- Control Plane: API Server, etcd, Scheduler, Controller Manager.
- Worker Node: kubelet, kube-proxy, container runtime (containerd/CRI-O).

## Вопрос 3
### Что такое Pod в Kubernetes?
Ответ: Наименьшая развертываемая единица — один или несколько контейнеров, разделяющих сеть/IP и тома. Эфемерный объект.

Пример:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    app: nginx
spec:
  containers:
  - name: nginx
    image: nginx:1.14.2
    ports:
    - containerPort: 80
```

## Вопрос 4
### В чем разница между Deployment и StatefulSet?
Ответ:
- Deployment: stateless, без стабильной идентичности Pod, порядок не важен.
- StatefulSet: stateful, стабильная идентичность (имена/тома), управляемый порядок.

Пример:
```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: mysql
spec:
  serviceName: mysql
  replicas: 3
  selector:
    matchLabels: { app: mysql }
  template:
    metadata: { labels: { app: mysql } }
    spec:
      containers:
      - name: mysql
        image: mysql:5.7
        ports: [{ containerPort: 3306 }]
        volumeMounts: [{ name: data, mountPath: /var/lib/mysql }]
  volumeClaimTemplates:
  - metadata: { name: data }
    spec:
      accessModes: ["ReadWriteOnce"]
      resources: { requests: { storage: 10Gi } }
```

## Вопрос 5
### Что такое Service и какие типы существуют?
Ответ: Абстракция доступа к группе Pod'ов со стабильным IP/DNS. Типы: ClusterIP, NodePort, LoadBalancer, ExternalName.

Пример:
```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  type: NodePort
  selector: { app: myapp }
  ports:
  - port: 80
    targetPort: 8080
    nodePort: 30080
```

## Вопрос 6
### Что такое Namespace?
Ответ: Логическая изоляция ресурсов в кластере (проекты/команды/окружения), для политик безопасности, квот и RBAC.

Пример:
```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: development
```

## Вопрос 7
### Что такое ConfigMap и Secret? Отличия?
Ответ: ConfigMap — неконфиденциальные настройки; Secret — конфиденциальные (base64), с ограничениями и шифрованием в etcd при включении.

Пример:
```yaml
apiVersion: v1
kind: ConfigMap
metadata: { name: app-config }
data:
  database_url: "mysql://db:3306"
  log_level: "info"
---
apiVersion: v1
kind: Secret
metadata: { name: app-secret }
type: Opaque
data:
  username: YWRtaW4=
  password: cGFzc3dvcmQ=
---
apiVersion: v1
kind: Pod
metadata: { name: myapp }
spec:
  containers:
  - name: app
    image: myapp:latest
    env:
    - name: DB_URL
      valueFrom:
        configMapKeyRef: { name: app-config, key: database_url }
    - name: DB_PASSWORD
      valueFrom:
        secretKeyRef: { name: app-secret, key: password }
```

## Вопрос 8
### Что такое Ingress?
Ответ: Правила внешнего HTTP/HTTPS-доступа к сервисам (SSL, vhost, path-based routing). Требует контроллер (Nginx, Traefik, Istio).

Пример:
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: app-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  ingressClassName: nginx
  rules:
  - host: myapp.example.com
    http:
      paths:
      - path: /api
        pathType: Prefix
        backend:
          service:
            name: api-service
            port: { number: 80 }
  tls:
  - hosts: [ myapp.example.com ]
    secretName: tls-secret
```

## Вопрос 9
### Что такое DaemonSet? Когда использовать?
Ответ: Гарантирует по 1 Pod на каждом (или выбранных) узле (агенты логов, мониторинга, CNI, безопасность).

Пример:
```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: fluentd
  namespace: kube-system
spec:
  selector: { matchLabels: { name: fluentd } }
  template:
    metadata: { labels: { name: fluentd } }
    spec:
      containers:
      - name: fluentd
        image: fluent/fluentd:v1.11
        volumeMounts: [{ name: varlog, mountPath: /var/log }]
      volumes:
      - name: varlog
        hostPath: { path: /var/log }
```

## Вопрос 10
### Что такое PersistentVolume и PersistentVolumeClaim?
Ответ: PV — ресурс хранения; PVC — запрос на хранилище. Режимы доступа: RWO, ROX, RWX.

Пример:
```yaml
apiVersion: v1
kind: PersistentVolume
metadata: { name: pv-data }
spec:
  capacity: { storage: 10Gi }
  accessModes: [ ReadWriteOnce ]
  persistentVolumeReclaimPolicy: Retain
  storageClassName: manual
  hostPath: { path: "/mnt/data" }
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata: { name: pvc-data }
spec:
  accessModes: [ ReadWriteOnce ]
  resources: { requests: { storage: 5Gi } }
  storageClassName: manual
---
apiVersion: v1
kind: Pod
metadata: { name: pod-with-pvc }
spec:
  containers:
  - name: app
    image: nginx
    volumeMounts: [{ name: storage, mountPath: "/usr/share/nginx/html" }]
  volumes:
  - name: storage
    persistentVolumeClaim: { claimName: pvc-data }
```

## Вопрос 11
### Что такое Liveness, Readiness и Startup пробы?
Ответ: Liveness перезапускает зависший контейнер; Readiness исключает Pod из Endpoints при неготовности; Startup — для медленного старта, блокирует запуск остальных проб до успешного старта.

Пример:
```yaml
apiVersion: v1
kind: Pod
metadata: { name: probe-example }
spec:
  containers:
  - name: app
    image: myapp:latest
    ports: [{ containerPort: 8080 }]
    startupProbe:
      httpGet: { path: /healthz, port: 8080 }
      failureThreshold: 30
      periodSeconds: 10
    livenessProbe:
      httpGet: { path: /healthz, port: 8080 }
      initialDelaySeconds: 15
      periodSeconds: 20
    readinessProbe:
      httpGet: { path: /ready, port: 8080 }
      initialDelaySeconds: 5
      periodSeconds: 10
```

## Вопрос 12
### Что такое RBAC в Kubernetes?
Ответ: Ролевая модель доступа: Role/ClusterRole + RoleBinding/ClusterRoleBinding + ServiceAccount. Принцип наименьших привилегий.

Пример:
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata: { namespace: default, name: pod-reader }
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get", "watch", "list"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata: { name: read-pods, namespace: default }
subjects:
- kind: ServiceAccount
  name: myapp-sa
  namespace: default
roleRef:
  kind: Role
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io
```

## Вопрос 13
### Что такое HorizontalPodAutoscaler (HPA)?
Ответ: Автомасштабирование реплик по метрикам (CPU, память, custom/external). Требуется metrics-server и корректные requests.

Пример:
```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata: { name: myapp-hpa }
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: myapp
  minReplicas: 2
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 75
  - type: Resource
    resource:
      name: memory
      target:
        type: Utilization
        averageUtilization: 80
```

## Вопрос 14
### Что такое ResourceQuota и LimitRange?
Ответ: ResourceQuota ограничивает ресурсы и количество объектов в namespace. LimitRange задает лимиты/requests по умолчанию и диапазоны на Pod/Container.

Пример:
```yaml
apiVersion: v1
kind: ResourceQuota
metadata: { name: compute-quota, namespace: development }
spec:
  hard:
    requests.cpu: "10"
    requests.memory: 20Gi
    limits.cpu: "20"
    limits.memory: 40Gi
    pods: "50"
    services: "20"
    persistentvolumeclaims: "10"
---
apiVersion: v1
kind: LimitRange
metadata: { name: resource-limits, namespace: development }
spec:
  limits:
  - max: { cpu: "2", memory: 4Gi }
    min: { cpu: "100m", memory: 128Mi }
    default: { cpu: "500m", memory: 512Mi }
    defaultRequest: { cpu: "200m", memory: 256Mi }
    type: Container
```

## Вопрос 15
### Что такое Rolling Update и как откатить?
Ответ: Постепенная замена Pod'ов. Настраивается maxUnavailable и maxSurge. Откат: kubectl rollout undo deployment/NAME.

Пример:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata: { name: myapp }
spec:
  replicas: 6
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxUnavailable: 1
      maxSurge: 2
  selector: { matchLabels: { app: myapp } }
  template:
    metadata: { labels: { app: myapp } }
    spec:
      containers:
      - name: myapp
        image: myapp:v2
```

## Вопрос 16
### Что такое NetworkPolicy?
Ответ: Политики сетевого доступа на уровне Pod (ingress/egress) по labels/портам/namespaceSelector. Требует CNI с поддержкой.

Пример:
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata: { name: allow-from-frontend, namespace: production }
spec:
  podSelector: { matchLabels: { app: backend } }
  policyTypes: [ Ingress ]
  ingress:
  - from:
    - podSelector: { matchLabels: { app: frontend } }
    ports:
    - protocol: TCP
      port: 8080
```

## Вопрос 17
### Jobs и CronJobs — как работают?
Ответ: Job гарантирует N успешных завершений; CronJob запускает Job по расписанию (cron). Управляем параллелизмом и backoff.

Пример:
```yaml
apiVersion: batch/v1
kind: Job
metadata: { name: backup-job }
spec:
  completions: 1
  parallelism: 1
  backoffLimit: 3
  template:
    spec:
      containers:
      - name: backup
        image: backup-tool:latest
        command: ["sh","-c","backup-database.sh"]
      restartPolicy: OnFailure
---
apiVersion: batch/v1
kind: CronJob
metadata: { name: daily-backup }
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

## Вопрос 18
### Что такое InitContainers?
Ответ: Специализированные контейнеры, запускаемые последовательно до основных контейнеров (ожидание зависимостей, подготовка данных, миграции).

Пример:
```yaml
apiVersion: v1
kind: Pod
metadata: { name: myapp-pod }
spec:
  initContainers:
  - name: wait-for-db
    image: busybox:1.28
    command: ["sh","-c","until nslookup mysql; do echo waiting; sleep 2; done;"]
  - name: clone-repo
    image: alpine/git
    command: ["git","clone","https://github.com/user/repo.git","/data"]
    volumeMounts: [{ name: data, mountPath: /data }]
  containers:
  - name: app
    image: myapp:latest
    volumeMounts: [{ name: data, mountPath: /app/data }]
  volumes:
  - name: data
    emptyDir: {}
```

## Вопрос 19
### Что такое Affinity и Anti-Affinity?
Ответ: NodeAffinity/Anti-Affinity — планирование по меткам узлов. PodAffinity/Anti-Affinity — совместное/раздельное размещение Pod'ов. required vs preferred.

Пример:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata: { name: web-app }
spec:
  replicas: 3
  selector: { matchLabels: { app: web } }
  template:
    metadata: { labels: { app: web } }
    spec:
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: disktype
                operator: In
                values: [ ssd ]
        podAntiAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
          - labelSelector:
              matchExpressions:
              - key: app
                operator: In
                values: [ web ]
            topologyKey: "kubernetes.io/hostname"
      containers:
      - name: web
        image: nginx
```

## Вопрос 20
### Что такое Taints и Tolerations?
Ответ: Taints на узлах «отталкивают» Pod'ы; Tolerations позволяют Pod'ам размещаться на «затаинченных» узлах. Эффекты: NoSchedule, PreferNoSchedule, NoExecute.

Пример:
```yaml
apiVersion: v1
kind: Pod
metadata: { name: gpu-pod }
spec:
  tolerations:
  - key: "gpu"
    operator: "Equal"
    value: "true"
    effect: "NoSchedule"
  containers:
  - name: app
    image: gpu-app:latest
```

## Вопрос 21
### Что такое Helm?
Ответ: Пакетный менеджер для Kubernetes: charts, releases, зависимости, версияция, откаты. В Helm 3 удален Tiller, улучшена безопасность, есть OCI-реестры.

## Вопрос 22
### Что такое Helm Chart и структура?
Ответ: Пакет Kubernetes-манифестов с шаблонами и значениями.

Пример:
```yaml
# Chart.yaml
apiVersion: v2
name: mychart
description: A Helm chart for Kubernetes
type: application
version: 0.1.0
appVersion: "1.16.0"
dependencies:
  - name: postgresql
    version: 11.6.12
    repository: https://charts.bitnami.com/bitnami
```

## Вопрос 23
### Как установить/обновить/удалить Helm chart?
Ответ: install/upgrade/rollback/uninstall, есть dry-run/debug и история релизов.

Команды:
```
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
helm install myrelease bitnami/nginx
helm upgrade myrelease bitnami/nginx --set replicaCount=3
helm history myrelease
helm rollback myrelease 2
helm uninstall myrelease
helm install myrelease ./mychart --dry-run --debug
```

## Вопрос 24
### Что такое values.yaml и переопределение значений?
Ответ: Значения по умолчанию. Приоритет: --set > -f/--values > values.yaml.

Пример:
```yaml
# values.yaml
replicaCount: 1
image: { repository: nginx, tag: latest }
# values-production.yaml
replicaCount: 3
image: { tag: "1.21.0" }
# Установка
# helm install myapp ./mychart -f values-production.yaml
# helm install myapp ./mychart --set replicaCount=5
```

## Вопрос 25
### Helm Templates и Go templating
Ответ: Используются .Values, .Release, .Chart, .Files, .Capabilities; if/else, range, functions, include.

Пример:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ include "mychart.fullname" . }}
  labels:
    {{- include "mychart.labels" . | nindent 4 }}
spec:
  {{- if not .Values.autoscaling.enabled }}
  replicas: {{ .Values.replicaCount }}
  {{- end }}
  template:
    spec:
      containers:
      - name: {{ .Chart.Name }}
        image: "{{ .Values.image.repository }}:{{ .Values.image.tag | default .Chart.AppVersion }}"
```

## Вопрос 26
### Что такое Helm Hooks?
Ответ: Kubernetes-ресурсы с аннотациями, запускающиеся на pre/post install/upgrade/delete/rollback, test; поддерживают вес и политику удаления.

Пример:
```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: {{ include "mychart.fullname" . }}-db-migration
  annotations:
    "helm.sh/hook": pre-install,pre-upgrade
    "helm.sh/hook-weight": "-5"
    "helm.sh/hook-delete-policy": before-hook-creation,hook-succeeded
spec:
  template:
    spec:
      restartPolicy: Never
      containers:
      - name: migration
        image: {{ .Values.migration.image }}
        command: ["sh","-c","run-migrations.sh"]
```

## Вопрос 27
### Как управлять зависимостями в charts?
Ответ: Указывать в Chart.yaml (dependencies), управлять через helm dependency list/update/build, conditions/tags для условной установки.

## Вопрос 28
### Helm Repository — работа с репозиториями
Ответ: Добавление/обновление/поиск, packaging, index; поддержка OCI.

Команды:
```
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo list
helm repo update
helm search repo nginx
helm package mychart
helm repo index . --url https://my-charts.example.com
helm registry login registry.example.com
helm push mychart-0.1.0.tgz oci://registry.example.com/charts
```

## Вопрос 29
### Как тестировать и отлаживать charts?
Ответ: helm lint, helm template, --dry-run --debug, helm test (test hooks), helm get.

Тест:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: "{{ include "mychart.fullname" . }}-test-connection"
  annotations: { "helm.sh/hook": test }
spec:
  containers:
  - name: wget
    image: busybox
    command: ['wget']
    args: ['{{ include "mychart.fullname" . }}:{{ .Values.service.port }}']
  restartPolicy: Never
```

## Вопрос 30
### Что такое Helmfile и зачем он нужен?
Ответ: Декларативное управление множеством релизов, окружениями, зависимостями; команды sync/apply/diff/destroy.

Пример helmfile.yaml (сокр.):
```yaml
repositories:
  - name: bitnami
    url: https://charts.bitnami.com/bitnami
releases:
  - name: postgresql
    namespace: database
    chart: bitnami/postgresql
    values: [ values/postgresql.yaml ]
  - name: myapp
    namespace: default
    chart: ./charts/myapp
    values: [ values/myapp-base.yaml, values/myapp-prod.yaml ]
    needs: [ database/postgresql ]
```

## Вопрос 31
### Различия Helm 2 и Helm 3
Ответ: Нет Tiller, релизы в namespaces, хранение в Secrets, Chart.yaml v2 с зависимостями, JSON Schema для values, поддержка OCI, улучшенные слияния, тип library для charts.

## Вопрос 32
### Управление секретами в Helm
Ответ: Helm Secrets (SOPS), Sealed Secrets, External Secrets Operator, Vault, переменные окружения/CI; не коммитить секреты в Git, шифровать sensitive файлы.

## Вопрос 33
### Helm в CI/CD
Ответ: Этапы: build, lint/template, package/publish, deploy; использовать --wait/--atomic, разные values по окружениям, auto-versioning.

## Вопрос 34
### Chart Hooks vs. Kubernetes Jobs
Ответ: Hooks привязаны к операциям Helm и могут авто-удаляться/упорядочиваться (weight). Jobs самостоятельны, без привязки к жизненному циклу релиза.

## Вопрос 35
### Library Chart — как создать?
Ответ: Chart с type: library, содержит вспомогательные шаблоны (_helpers.tpl), ставится зависимостью и используется через include.

## Вопрос 36
### ChartMuseum — как использовать?
Ответ: Open-source сервер репозитория charts (локально/S3/GCS/Azure). Альтернативы: Harbor, Artifactory, Nexus.

Команды:
```
helm repo add chartmuseum https://chartmuseum.github.io/charts
helm install chartmuseum chartmuseum/chartmuseum --set env.open.DISABLE_API=false --set persistence.enabled=true
curl --data-binary "@mychart-0.1.0.tgz" http://chartmuseum.example.com/api/charts
```

## Вопрос 37
### Kubernetes DNS и Service Discovery
Ответ: CoreDNS. Имена вида service.namespace.svc.cluster.local; headless Service создает A-записи на Pod'ы. Рекомендуемый способ — DNS.

## Вопрос 38
### Operators и CRD
Ответ: CRD + контроллер с domain knowledge для управления приложениями (миграции, бэкапы, обновления). Инструменты: Operator SDK, Kubebuilder.

## Вопрос 39
### Мониторинг Prometheus + Grafana
Ответ: kube-prometheus-stack (Prometheus, Alertmanager, Grafana, exporters). ServiceMonitor/PodMonitor для сбора метрик приложений, PrometheusRule для алертов.

## Вопрос 40
### Blue-Green и Canary деплойменты
Ответ:
- Blue-Green: моментальное переключение трафика между средами, простой откат, требуется больше ресурсов.
- Canary: поэтапное включение трафика, мониторинг метрик, меньше ресурсов, сложнее настройка. Инструменты: Argo Rollouts, Flagger, Istio.

## Вопрос 41
### Scheduler и влияние на планирование
Ответ: Два этапа — фильтрация и оценка узлов. Влияние: nodeSelector, Affinity/Anti-Affinity, Taints/Tolerations, PriorityClass, topology spread constraints.

## Вопрос 42
### Kubernetes Volumes — типы
Ответ: Ephemeral (emptyDir, configMap, secret, downwardAPI), Persistent (PVC/CSI/NFS/hostPath), Cloud-specific (EBS, PD, Azure Disk/File).

## Вопрос 43
### Безопасность контейнеров
Ответ: Pod Security Standards (restricted), securityContext (runAsNonRoot, RO FS, capabilities), NetworkPolicy, image scanning (Trivy), RBAC, отдельные SA.

## Вопрос 44
### Admission Controllers
Ответ: Mutating/Validating плагины в цепочке приема запроса; webhooks (MutatingWebhookConfiguration/ValidatingWebhookConfiguration), OPA/Gatekeeper, sidecar injection.

## Вопрос 45
### Garbage Collection
Ответ: Удаление «мусора»: orphaned/failed/completed pods, unused images; ownerReferences и каскадные политики (Foreground/Background/Orphan), TTL Jobs, история CronJob.

## Вопрос 46
### Custom Resource Definitions (CRD)
Ответ: Расширяют Kubernetes API (schema/версионирование/subresources/конверсия). Контроллер управляет жизненным циклом CR.

## Вопрос 47
### etcd — как работает и зачем?
Ответ: Консистентное key-value хранилище (Raft); хранит состояние кластера и объекты. Практики: 3–5 узлов, регулярные бэкапы, шифрование at rest, SSD, мониторинг.

## Вопрос 48
### Resource Requests и Limits
Ответ: Requests — для планирования; Limits — предельные значения (CPU throttling, OOMKill). QoS: Guaranteed, Burstable, BestEffort. Рекомендации: всегда задавать requests, memory limits обязательны.

## Вопрос 49
### Service Mesh и Istio
Ответ: Управление сетевым взаимодействием: traffic management, mTLS, observability, resilience. Компоненты: Envoy (data plane), Istiod (control plane). Альтернативы: Linkerd, Consul, AWS App Mesh.

## Вопрос 50
### Multi-tenancy в Kubernetes
Ответ: Namespace-based (мягкая изоляция: RBAC, NetworkPolicy, Quota, PSS), Cluster-based (жесткая изоляция: отдельные кластеры), Virtual Clusters (vCluster). Практики: taints/nodepools, аудит/мониторинг.