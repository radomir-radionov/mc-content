# Kubernetes — собеседование

Ответы заполнены.

[← К обзору](./0_system_design.md)

### Related docs

- [Kubernetes — собеседование](./0_sd_kubernetes_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — Kubernetes](https://devinterview.io/questions/software-architecture-and-system-design/kubernetes-interview-questions/).

### Kubernetes Basics

<details>
<summary>Что такое Kubernetes, and why is it used for container orchestration?</summary>

### Что такое Kubernetes, and why is it used for container orchestration?

**Definition:**

Kubernetes — это управляющий слой, который запускает контейнеры на кластере и держит их в объявленном состоянии. Клиенты ходят по стабильному имени, пока поды переносятся, перезапускаются или заменяются.

**How it works:**

- Планируется Pod: один или несколько контейнеров с общим сетевым пространством и томами. Deployment и ReplicaSet держат заданное число таких подов.
- У каждого пода свой IP. Service, Ingress и NetworkPolicy стоят над этим, чтобы клиенты не следили за адресами подов.
- PersistentVolume, claim и StorageClass подключают диски, которые живут дольше пода, в том числе созданные по запросу.
- Horizontal Pod Autoscaler меняет число реплик по CPU или памяти. Cluster autoscaler добавляет или убирает узлы.
- ResourceQuota и LimitRange ограничивают CPU и память для пространства имён или контейнера.
- Job и CronJob доводят пакетную задачу до конца, а не держат её как сервер.
- Rolling update меняет поды пачками. PodDisruptionBudget ограничивает, сколько их может быть недоступно во время обслуживания.
- Проваленный liveness probe перезапускает контейнер. Проваленный readiness probe убирает под из трафика.
- Secret и ConfigMap передают секреты и настройки, не зашивая их в образ.
- Метки выбирают группы объектов. Аннотации несут дополнительные данные, которые селекторы не читают.
- Свои контроллеры могут следить за API и приводить к нужному виду объявленные вами типы.
- Тот же API работает на ноутбуке, в своём дата-центре и в облаке. Инструмент вроде KubeFed может разослать общую конфигурацию на несколько кластеров.
- Уплотнение нагрузки на узлах снижает запас простаивающих ресурсов, а одни и те же манифесты выкатываются в любой среде.

**Examples:**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: checkout
spec:
  containers:
    - name: api
      image: checkout:1.4
    - name: logs
      image: log-shipper:2
```

Контейнер API и сборщик логов ставятся вместе и видят друг друга на localhost.
</details>

<details>
<summary>Опишите the roles of master and worker nodes in the Kubernetes architecture.</summary>

### Опишите the roles of master and worker nodes in the Kubernetes architecture.

**Definition:**

Кластер делится на управляющий слой, который решает, что должно работать, и рабочие узлы, на которых работают контейнеры.

**How it works:**

- Управляющий слой, его всё ещё часто называют master, запускает API server, scheduler и controller manager. etcd хранит состояние кластера и часто живёт отдельным отказоустойчивым набором.
- API server — единственная входная дверь. С ним говорят kubectl и все контроллеры.
- Scheduler выбирает рабочий узел, где хватает CPU, памяти и совпадают ограничения.
- Контроллеры смотрят на живые объекты и создают или удаляют поды, пока кластер не совпадёт со спецификацией.
- Эти процессы можно держать на нескольких машинах управляющего слоя. Пользовательские контейнеры обычно туда не ставят.
- На каждом рабочем узле работают kubelet, который запускает контейнеры назначенных подов, и kube-proxy, который настраивает пересылку Service.
- Среда исполнения на узле (containerd или CRI-O) и создаёт процессы.
- Иногда выделяют машины только под хранилище или ingress. Это дополнение, а не обязательная третья роль.

**Examples:**

`kubectl get nodes` помечает машины управляющего слоя taint control-plane, остальные остаются рабочими. Новый под сохраняет API server, размещает scheduler и запускает kubelet на выбранном узле.
</details>

<details>
<summary>Как Pods facilitate the management of containers in Kubernetes?</summary>

### Как Pods facilitate the management of containers in Kubernetes?

**Definition:**

Pod — единица, которую планировщик ставит на узел. Он объединяет один или несколько контейнеров, которые должны работать вместе и делить сеть и тома.

**How it works:**

- Контейнеры в поде делят один IP, одно имя хоста и одно сетевое пространство, поэтому достучаться друг до друга можно через localhost.
- Под планируется целиком: все его контейнеры попадают на один узел или не попадает ни один.
- Тома, объявленные на поде, можно смонтировать в каждый его контейнер.
- У каждого контейнера всё равно свой образ, команда и запрос CPU или памяти.
- Под с одним контейнером — обычный случай для самостоятельного процесса.
- Дополнительные контейнеры — sidecar: прокси, сборщик логов или синхронизация файлов рядом с основным процессом.
- kubelet может перезапустить контейнер по политике перезапуска пода. Идентичность пода сохраняется, пока не заменят сам под.
- Deployment, Job и DaemonSet не планируют контейнеры напрямую. Они штампуют поды из шаблона.

**Examples:**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: web
spec:
  containers:
    - name: nginx
      image: nginx:1.27
      ports:
        - containerPort: 80
    - name: metrics
      image: exporter:1
```

Экспортёр обращается к nginx по `127.0.0.1:80`, потому что оба контейнера делят сеть пода.
</details>

<details>
<summary>Какие типы Services есть в Kubernetes и как они обеспечивают pod communication?</summary>

### Какие типы Services есть в Kubernetes и как они обеспечивают pod communication?

**Definition:**

Service даёт стабильное имя и адрес для меняющегося набора подов. Тип решает, кто может пользоваться этим адресом.

**How it works:**

- ClusterIP, тип по умолчанию, выделяет виртуальный IP внутри кластера и балансирует по готовым подам, которые совпали с селектором.
- NodePort дополнительно открывает этот порт на каждом узле, чтобы трафик снаружи попадал внутрь и уходил на эти поды.
- LoadBalancer просит облако или контроллер поставить внешний балансировщик перед Service.
- ExternalName публикует CNAME на DNS-имя вне кластера и не выбирает поды.
- Headless Service (`clusterIP: None`) возвращает IP самих подов. StatefulSet использует это для стабильного DNS каждого пода.
- Селектор заполняет EndpointSlice. Поды, не прошедшие readiness, выпадают из списка, и клиенты держат один адрес, пока состав меняется.

**Examples:**

```yaml
apiVersion: v1
kind: Service
metadata:
  name: payments
spec:
  type: ClusterIP
  selector:
    app: payments
  ports:
    - port: 80
      targetPort: 8080
```

Клиенты ходят на `http://payments` и не запоминают IP пода.
</details>

<details>
<summary>Объясните the functionality of Namespaces in Kubernetes.</summary>

### Объясните the functionality of Namespaces in Kubernetes.

**Definition:**

Namespace — граница имён и политик внутри одного кластера, чтобы команды или среды делили машины без столкновения имён.

**How it works:**

- Имена подов, Service и большинства других объектов должны быть уникальны только внутри Namespace. `api` в `dev` и `api` в `prod` — разные объекты.
- ResourceQuota ограничивает суммарные CPU, память и число объектов. LimitRange подставляет запросы по умолчанию, если контейнер их не указал.
- Привязки ролей можно ограничить одним Namespace, а NetworkPolicy — тем, какие пространства имён могут общаться.
- DNS включает Namespace (`api.dev.svc.cluster.local`). Короткое имя `api` разрешается только в том же Namespace.
- Само по себе пространство имён не делит узлы и физическую сеть. Границу делают квоты и политики.
- Node и PersistentVolume относятся ко всему кластеру и не живут в Namespace.

**Examples:**

`kubectl create namespace staging`, затем `kubectl -n staging apply -f app.yaml` держит эти объекты вне `default`. Квота в 4 CPU в `staging` отклонит следующий под, который её превысит.
</details>

### Kubernetes Objects and Workloads

<details>
<summary>Различите between Deployments, StatefulSets, and DaemonSets.</summary>

### Различите between Deployments, StatefulSets, and DaemonSets.

**Definition:**

Deployment, StatefulSet и DaemonSet все поддерживают поды запущенными. Они различаются идентичностью, хранилищем и тем, куда ставятся поды.

**How it works:**

- Deployment владеет ReplicaSet из взаимозаменяемых подов. Любая реплика может обслужить любой запрос, хранилище обычно внешнее, а выкат заменяет поды пачками. Соседи общаются через Service, а не через стабильные имена подов.
- StatefulSet даёт каждому поду стабильный порядковый номер (`db-0`, `db-1`) и обычно свой том через `volumeClaimTemplates`. Он создаёт и удаляет их по порядку. Headless DNS сохраняет эти имена после перезапуска.
- DaemonSet запускает один под на каждом выбранном узле, включая узлы, которые присоединились позже. Числа реплик для масштабирования нет, и подам редко нужно находить друг друга. Так ставят агентов узла, например сборщики логов.
- Deployment — для реплик без состояния, StatefulSet — когда идентичность и диск должны пережить перенос, DaemonSet — когда работа нужна на каждом узле.
- Разовая задача — это Job, а не один из этих трёх.

**Examples:**

Три реплики веба — Deployment. База, где `db-0` основной, — StatefulSet. Сборщик логов на каждой машине — DaemonSet.
</details>

<details>
<summary>Как ReplicaSets ensure pod availability?</summary>

### Как ReplicaSets ensure pod availability?

**Definition:**

ReplicaSet держит число работающих подов равным объявленному, создавая замены из шаблона пода.

**How it works:**

- Шаблон — это спецификация пода, которую копируют для каждой новой реплики, включая метки и образ контейнера.
- Контроллер сравнивает поды, совпавшие с селектором, с `spec.replicas`.
- Если под исчез или его узел потерян, контроллер создаёт другой из шаблона.
- Увеличение или уменьшение `replicas` масштабирует набор.
- Deployment создаёт новый ReplicaSet на каждый выкат и переносит реплики со старого набора на новый. Откат — это то же масштабирование в обратную сторону.
- Селектор должен совпадать с метками шаблона. Иначе контроллер не примет поды, которые сам создал.

**Examples:**

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: cache
spec:
  replicas: 2
  selector:
    matchLabels:
      app: cache
  template:
    metadata:
      labels:
        app: cache
    spec:
      containers:
        - name: redis
          image: redis:7
```

Удалите один под — появится новый под `redis:7` без ручного перезапуска.
</details>

<details>
<summary>Что такое Jobs in Kubernetes, and when is it suitable to use them?</summary>

### Что такое Jobs in Kubernetes, and when is it suitable to use them?

**Definition:**

Job запускает поды, пока заданное число не завершится успешно, и затем останавливается. Он подходит для пакетной работы, а не для процесса, который должен жить постоянно.

**How it works:**

- `completions` — сколько успешных подов нужно. `parallelism` — сколько может работать одновременно.
- Под, вышедший с кодом 0, не перезапускается. Упавший под можно повторить до `backoffLimit`.
- Завершённые поды остаются, пока лимит истории или TTL-контроллер их не удалит.
- `completions: 1` и `parallelism: 1` — один запуск, например миграция схемы.
- Большие числа гоняют независимые куски параллельно: сборку пакетов или обработку файлов.
- Job с очередью позволяет воркерам забирать задачи, пока очередь не опустеет. Часто используют индексы, чтобы каждый индекс был отдельным элементом.
- CronJob создаёт Job по расписанию для резервных копий и другой повторяющейся пакетной работы.

**Examples:**

Ночной экспорт — CronJob, в шаблоне которого `completions: 1`. Импорт из 20 частей: `completions: 20` и `parallelism: 4`, четыре воркера работают, пока двадцать не завершатся успешно.
</details>

<details>
<summary>Как Labels and Selectors work together in Kubernetes?</summary>

### Как Labels and Selectors work together in Kubernetes?

**Definition:**

Метки — пары ключ-значение на объектах. Селекторы — запросы, которые находят объекты по этим меткам, поэтому Service или контроллер не зашивает имена подов.

**How it works:**

- Метки вроде `app` или `env` обозначают группу. Это не произвольные заметки.
- `matchLabels` требует каждую перечисленную пару. Селекторы по множеству добавляют `In`, `NotIn` и `Exists`.
- Service, ReplicaSet, Deployment, DaemonSet, Job, StatefulSet и NetworkPolicy выбирают поды именно так.
- Селектор Service заполняет EndpointSlice. Ingress направляет на Service, а не прямо по метке пода.
- Смена метки перемещает объект в группу или из неё без правки потребителя.
- Аннотации держат дополнительные данные, которые селекторы игнорируют.
- Service без селектора не собирает endpoints сам. Пустой селектор не значит «совпасть со всем».

**Examples:**

```yaml
metadata:
  labels:
    app: shop
    env: prod
---
spec:
  selector:
    matchLabels:
      app: shop
      env: prod
```

Deployment, Service и NetworkPolicy могут использовать одну и ту же пару.
</details>

<details>
<summary>Что бы вы you consider when performing a rolling update in Kubernetes?</summary>

### Что бы вы you consider when performing a rolling update in Kubernetes?

**Definition:**

Rolling update заменяет старые поды новыми небольшими порциями, чтобы часть мощности оставалась доступной всё время.

**How it works:**

- Deployment создаёт новый ReplicaSet и наращивает его, одновременно уменьшая предыдущий.
- `maxUnavailable` — насколько число готовых подов может опуститься ниже желаемого. `maxSurge` — сколько лишних подов допустимо во время замены.
- Новый под получает трафик только после успешного readiness probe.
- Проваленный liveness probe перезапускает этот контейнер. Если новый ReplicaSet так и не станет готовым, выкат останавливается, и можно вернуть масштаб предыдущему ReplicaSet.
- `kubectl rollout status` показывает прогресс. PodDisruptionBudget тоже может отказать в добровольном выселении, если слишком много подов окажется недоступно.
- Обе версии обслуживают сразу, поэтому новый релиз должен терпеть ещё работающую старую версию.

**Examples:**

```yaml
strategy:
  type: RollingUpdate
  rollingUpdate:
    maxUnavailable: 0
    maxSurge: 1
```

При трёх репликах Kubernetes сначала поднимает один новый под, ждёт готовности и только потом убирает один старый.
</details>

### Kubernetes Networking

<details>
<summary>Как Services route traffic to pods in Kubernetes?</summary>

### Как Services route traffic to pods in Kubernetes?

**Definition:**

Service не отправляет пакеты на метку. Он хранит список адресов готовых подов, а узел перенаправляет IP Service на один из этих адресов.

**How it works:**

- Селектор совпадает с метками подов. Управляющий слой записывает IP и порты готовых подов в EndpointSlice.
- kube-proxy или другой data plane ставит правила iptables, IPVS или nftables, чтобы ClusterIP транслировался на бэкенд.
- Поды, не прошедшие readiness, выходят из списка, и трафик прекращается до удаления процесса.
- `sessionAffinity: ClientIP` привязывает адрес клиента к одному поду на время таймаута. `None`, значение по умолчанию, может выбрать другой бэкенд на каждое соединение. Это привязка по адресу, а не cookie.
- ClusterIP, NodePort и LoadBalancer используют один и тот же список endpoints. ExternalName только возвращает DNS CNAME.
- `port` — то, чем пользуются клиенты на Service. `targetPort` — порт контейнера.

**Examples:**

```yaml
spec:
  selector:
    app: shop
  sessionAffinity: ClientIP
  ports:
    - port: 80
      targetPort: 8080
```

Соединение с ClusterIP на порт 80 попадает на порт 8080 одного готового пода с меткой `app=shop`.
</details>

<details>
<summary>Опишите the puRPOse of Ingress and its key components.</summary>

### Опишите the puRPOse of Ingress and its key components.

**Definition:**

Ingress описывает маршрутизацию HTTP и HTTPS снаружи кластера на Service по хосту и пути. Объект ничего не делает, пока его не реализует контроллер.

**How it works:**

- Ресурс Ingress перечисляет хосты, пути, бэкенд-Service и при необходимости TLS Secret.
- Ingress-контроллер (NGINX, Traefik или облачный контроллер) следит за этими объектами и настраивает прокси или балансировщик седьмого уровня.
- Много Service могут делить один внешний адрес и один сертификат.
- Правило может отправить `shop.example` `/api` на один Service, а `/` на другой.
- Это подходит кластерам, которые отдают несколько HTTP-сервисов через один вход. Трафик не HTTP по-прежнему идёт через Service типа LoadBalancer или NodePort.
- Gateway API — более новая модель той же задачи, с более ясными ролями и местом для разделения трафика.

**Examples:**

Один хост, два пути: `/` идёт на `web:80`, `/pay` на `payments:80`, TLS берётся из Secret `shop-tls`. Клиенты ходят на `https://shop.example` и не видят ClusterIP.
</details>

<details>
<summary>Объясните Kubernetes DNS resolution for services.</summary>

### Объясните Kubernetes DNS resolution for services.

**Definition:**

В кластере работает DNS-сервер, обычно CoreDNS, который сопоставляет имена Service и подов с адресами, чтобы нагрузки звали друг друга по имени.

**How it works:**

- Ранние кластеры использовали kube-dns. Его сменил CoreDNS, который включает возможности плагинами.
- Service `api` в Namespace `shop` — это `api.shop.svc.cluster.local`. Внутри этого Namespace достаточно короткого имени `api`.
- Поды с `dnsPolicy: ClusterFirst` отправляют кластерные имена в CoreDNS, а остальные — выше по цепочке. `dnsConfig` добавляет домены поиска или дополнительные серверы.
- Обычный Service разрешается в свой ClusterIP. Headless Service разрешается в IP подов.
- CoreDNS следит за API, поэтому запись следует за сменой адреса.
- То же короткое имя в другом Namespace — другой Service. Вызов через границу использует полное имя.
- Имён между кластерами из коробки нет. Их дают внешний DNS, Ingress или mesh.

**Examples:**

Из пода в `shop` команда `nslookup api` возвращает ClusterIP. `nslookup api.billing.svc.cluster.local` попадает в billing. Headless-имя StatefulSet `db-0.db.shop.svc.cluster.local` возвращает IP этого пода.
</details>

<details>
<summary>Что такое the container Network interface (CNI), and how does it integrate with Kubernetes?</summary>

### Что такое the container Network interface (CNI), and how does it integrate with Kubernetes?

**Definition:**

CNI — интерфейс плагина, через который kubelet подключает под к сети. Саму сеть подов Kubernetes не реализует. Это делает установленный плагин.

**How it works:**

- Когда среда исполнения создаёт песочницу пода, kubelet вызывает бинарник CNI, настроенный в `/etc/cni/net.d`.
- Плагин выдаёт IP, подключает veth или аналог и прописывает маршруты. При удалении он их снимает.
- Calico, Cilium и Flannel — частые плагины. Они различаются оверлеем и маршрутизируемой сетью и тем, применяют ли они NetworkPolicy.
- Плагин обычно ставят как DaemonSet, чтобы он работал на каждом узле.
- Контейнеры одного пода уже делят пространство имён. CNI соединяет это пространство с остальным кластером.
- Multus — необязательный мета-плагин, который может добавить интерфейсы через NetworkAttachmentDefinition. Обычный путь — один интерфейс.
- Service и Ingress стоят над сетью подов. CNI даёт достижимость. NetworkPolicy решает, какие потоки разрешены.

**Examples:**

Когда под в состоянии Running, `kubectl get pod -o wide` показывает IP, который выдал плагин. Если DaemonSet CNI лежит, новые поды остаются в ContainerCreating с ошибкой сетевого плагина.
</details>

<details>
<summary>Как you implement Network Policies, and what are their benefits?</summary>

### Как you implement Network Policies, and what are their benefits?

**Definition:**

NetworkPolicy выбирает поды и перечисляет, какие соседи, порты и протоколы могут подключаться. Она ограничивает боковое движение, когда контроллер реально её применяет.

**How it works:**

- `podSelector` выбирает поды. Правила ingress перечисляют, кто может подключиться внутрь. Правила egress — куда эти поды могут подключаться наружу.
- Сосед может быть другими подами по метке, другим Namespace или блоком IP. Порты и протоколы сужают совпадение.
- В основном API, как только политика выбирает под для направления, это направление становится списком разрешений. Трафик, который ни одна политика не разрешила, отбрасывается. Поды без политики остаются открытыми.
- Объекты ничего не делают, пока плагин вроде Calico или Cilium их не применяет.
- Эти плагины добавляют и более богатые правила, включая явный запрет. Cilium может сопоставлять идентичность приложения, а не только IP и порты.
- Правила добавляют маленькими шагами и проверяют. Политика ingress без разрешающего правила запирает под.
- Политики фиксируют ожидаемые потоки и дают ревью конкретный список разрешений.

**Examples:**

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: api-in
spec:
  podSelector:
    matchLabels:
      app: api
  policyTypes: [Ingress]
  ingress:
    - from:
        - podSelector:
            matchLabels:
              app: web
      ports:
        - protocol: TCP
          port: 8080
```

Только поды с меткой `app=web` могут открыть TCP 8080 к `app=api`. Остальной входящий трафик запрещён.
</details>

### Kubernetes Storage

<details>
<summary>Как PersistentVolumes (PVs) and PersistentVolumeClaims (PVCs) interact in Kubernetes?</summary>

### Как PersistentVolumes (PVs) and PersistentVolumeClaims (PVCs) interact in Kubernetes?

**Definition:**

PersistentVolume — это хранилище кластера со своим жизненным циклом. PersistentVolumeClaim — запрос пользователя на объём и режим доступа. Управляющий контур связывает одну заявку с одним томом.

**How it works:**

- Администратор создаёт PV заранее, либо provisioner создаёт том под заявку. PV — ресурс кластера, как и узел, и он переживает отдельный Pod.
- Заявка просит ёмкость и режим доступа: ReadWriteOnce, ReadOnlyMany, ReadWriteMany или ReadWriteOncePod. Цикл управления связывает её с подходящим PV. Связь один к одному через ClaimRef.
- Pod не называет PV. Он монтирует заявку. Кластер находит связанный том и монтирует его.
- Если подходящего тома нет, заявка остаётся несвязанной. PV на 50Gi не закрывает заявку на 100Gi.
- Storage Object in Use Protection не даёт сразу удалить PVC, которую ещё использует Pod, и PV, всё ещё связанный с PVC.

**When to use:**

Заявку берут, когда данные диска должны пережить перезапуск Pod. emptyDir подходит, когда данные могут исчезнуть вместе с Pod.

**Examples:**

```text
PVC просит 30Gi ReadWriteOnce
контроллер связывает подходящий PV
Pod volumes.persistentVolumeClaim.claimName = эта PVC
```

**Sources:**

- https://kubernetes.io/docs/concepts/storage/persistent-volumes/
</details>

<details>
<summary>Объясните dynamic provisioning in relation to StorageClasses.</summary>

### Объясните dynamic provisioning in relation to StorageClasses.

**Definition:**

Динамическое выделение создаёт PersistentVolume, когда заявка просит StorageClass и статический том не подходит. Класс — это предложение администратора определённого типа хранилища.

**How it works:**

- StorageClass называет provisioner и параметры, например производительность. Заявка ставит storageClassName в имя этого класса.
- Пустое имя класса отключает динамическое выделение для этой заявки.
- Привязка бывает Immediate, это значение по умолчанию, или WaitForFirstConsumer: том ждёт Pod, который использует заявку, чтобы учесть топологию.
- Динамический том наследует политику возврата своего StorageClass. По умолчанию это Delete: удаление заявки удаляет том. Retain оставляет том в фазе Released, чтобы администратор мог забрать данные.
- Admission-плагин DefaultStorageClass может подставить класс по умолчанию, если заявка его не указала.

**When to use:**

Динамическое выделение берут, когда пользователи сами просят размер и класс, без заранее созданных дисков. Статические PV — когда диск уже существует вне кластера.

**Examples:**

```yaml
storageClassName: fast
accessModes: [ReadWriteOnce]
resources:
  requests:
    storage: 30Gi
```

**Sources:**

- https://kubernetes.io/docs/concepts/storage/dynamic-provisioning/
- https://kubernetes.io/docs/concepts/storage/storage-classes/
- https://kubernetes.io/docs/concepts/storage/persistent-volumes/
</details>

<details>
<summary>Какие стратегии exist for handling persistent data when a pod’s lifecycle ends?</summary>

### Какие стратегии exist for handling persistent data when a pod’s lifecycle ends?

**Definition:**

Удаление Pod не удаляет связанный PersistentVolume. Судьба байтов зависит от заявки, политики возврата и от того, держит ли контроллер вроде StatefulSet эту заявку.

**How it works:**

- Том, смонтированный из заявки, остаётся после исчезновения Pod. Следующий Pod с той же заявкой видит те же данные.
- Удаление PVC запускает возврат. Retain оставляет PV в Released и сохраняет внешний диск. Delete удаляет PV и, если плагин умеет, внешний ресурс. Recycle устарел; вместо него рекомендуют динамическое выделение.
- Защита объекта откладывает удаление, пока Pod ещё использует заявку или PV ещё связан.
- Уменьшение StatefulSet удаляет Pod, но оставляет их PVC связанными, поэтому данные на месте, когда порядковый номер возвращается.

**When to use:**

Retain — когда данные ценные и решение принимает человек. Delete — когда жизнь данных равна жизни заявки. Заявка StatefulSet на каждый порядковый номер — когда личность и диск должны держаться вместе.

**Sources:**

- https://kubernetes.io/docs/concepts/storage/persistent-volumes/
- https://kubernetes.io/docs/tutorials/stateful-application/basic-stateful-set/
</details>

### Kubernetes Cluster Management

<details>
<summary>Опишите the process for initializing a Kubernetes cluster.</summary>

### Опишите the process for initializing a Kubernetes cluster.

**Definition:**

Кластер — это управляющий контур плюс рабочие узлы. Инициализация поднимает API, хранилище состояния, планировщик и контроллеры, затем узлы, которые могут запускать Pod.

**How it works:**

- Управляющий контур: kube-apiserver (HTTP API), etcd (согласованное хранилище данных API), kube-scheduler (привязывает ещё не назначенные Pod к узлам) и kube-controller-manager (запускает контроллеры). cloud-controller-manager необязателен.
- На каждом узле работает kubelet, который поддерживает Pod и их контейнеры, среда выполнения контейнеров и при желании kube-proxy для правил Service.
- Дополнения вроде кластерного DNS не входят в это ядро, но рабочий кластер обычно ставит их после того, как API отвечает.
- Документация предлагает minikube или учебный playground, если нужен кластер только чтобы попробовать API. Промышленная схема — это выбор того, как развернуть те же компоненты.

**When to use:**

Одноузловой локальный кластер достаточен, чтобы учить API. Несколько машин управляющего контура и высокодоступный etcd — форма кластера, от которого зависят другие люди.

**Sources:**

- https://kubernetes.io/docs/concepts/overview/components/
- https://kubernetes.io/docs/tasks/extend-kubernetes/custom-resources/custom-resource-definitions/
</details>

<details>
<summary>Какие факторы учитывать при добавлении new nodes в Kubernetes cluster?</summary>

### Какие факторы учитывать при добавлении new nodes в Kubernetes cluster?

**Definition:**

Новый узел входит в кластер, когда на нём работают компоненты узла и планировщик может ставить на него Pod. Ёмкость, метки и taint решают, что туда реально попадёт.

**How it works:**

- Узлу нужны kubelet и среда выполнения контейнеров. kube-proxy необязателен и держит сетевые правила Service.
- Планировщик ставит Pod, которым хватает свободных CPU и памяти. Узел без запаса не получит Pod с большим запросом.
- Метки позволяют потом целиться в узел через nodeSelector или affinity. Для границы безопасности берите ключи, которые kubelet не может переписать, например префикс node-restriction.kubernetes.io/.
- Осушенный узел несёт taint unschedulable. Новые Pod без toleration туда не встанут. Обычное исключение — DaemonSet.
- Добавление узлов само по себе не копирует данные управляющего контура. Состояние кластера остаётся в etcd и API-сервере.

**Sources:**

- https://kubernetes.io/docs/concepts/overview/components/
- https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/
- https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/
</details>

<details>
<summary>Как draining a node affect the running applications, and when should you perform this action?</summary>

### Как draining a node affect the running applications, and when should you perform this action?

**Definition:**

Осушение выселяет Pod с узла, чтобы можно было обслуживать машину. Нагрузки с PodDisruptionBudget остаются доступны только в пределах заданного бюджета.

**How it works:**

- kubectl drain выселяет Pod, соблюдает их grace period и PodDisruptionBudget. Когда команда успешно завершается, выселенные Pod исчезли, а узел не принимает новые.
- Pod DaemonSet контроллер тут же создаёт заново, и они игнорируют taint unschedulable. Без --ignore-daemonsets drain не закончит их за вас.
- Pod, у которого в spec задан nodeName, привязан к этому узлу и может продолжать работать там после осушения.
- Так делают перед обслуживанием ядра, железа или виртуальной машины. После обслуживания kubectl uncordon снова разрешает планирование.
- Одна команда drain — один узел. Параллельные drain разных узлов всё равно чтут тот же бюджет: StatefulSet с minAvailable 2 не опустится ниже.

**When to use:**

Осушайте узел перед перезагрузкой или удалением. Не осушайте, если оставшиеся реплики не укладываются в бюджет нарушений.

**Examples:**

```text
kubectl drain --ignore-daemonsets <node name>
# обслуживание
kubectl uncordon <node name>
```

**Sources:**

- https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/
</details>

<details>
<summary>В what scenarios would you scale a Kubernetes deployment, and what tools would you use?</summary>

### В what scenarios would you scale a Kubernetes deployment, and what tools would you use?

**Definition:**

Масштабируйте Deployment, когда спрос больше не совпадает с числом реплик или с CPU и памятью каждой реплики. Kubernetes меняет число (горизонтально) или ресурсы (вертикально), вручную или контроллером.

**How it works:**

- Ручной горизонтальный масштаб меняет число реплик. Ручной вертикальный — патчит запросы ресурсов контейнера.
- HorizontalPodAutoscaler — встроенный ресурс и контроллер. Он периодически ставит число реплик по наблюдаемой утилизации, например CPU или памяти.
- VerticalPodAutoscaler — дополнение, не ядро Kubernetes. Ему нужен Metrics Server. По документации автомасштабирования он пока не меняет размер Pod на месте; изменение ресурсов контейнера на месте — отдельная стабильная возможность, её применяют вручную.
- KEDA, проект CNCF, масштабирует по событиям вроде глубины очереди или по расписанию cron. Пропорциональные автоскейлеры кластера смотрят на число узлов или ядер и тоже живут отдельными проектами.
- Если реплики всё равно не помещаются, масштабируют сам кластер: добавляют или убирают узлы.

**When to use:**

HPA — когда нагрузка это CPU, память или метрика, которая следует за частотой запросов. KEDA — когда сигнал это очередь или расписание. Больше узлов — когда заполнен сам кластер.

**Sources:**

- https://kubernetes.io/docs/concepts/workloads/autoscaling/
</details>

### Kubernetes Security

<details>
<summary>Объясните Role-BASEd Access Control (RBAC) in Kubernetes.</summary>

### Объясните Role-BASEd Access Control (RBAC) in Kubernetes.

**Definition:**

RBAC Kubernetes выдаёт пользователю или ServiceAccount только те глаголы API, которые нужны роли. Role и привязки — объекты, которыми это выражают.

**How it works:**

- Предпочитайте Role и RoleBinding в одном пространстве имён. ClusterRoleBinding даёт те же права во всех пространствах.
- Избегайте шаблонов, особенно на все ресурсы. Kubernetes расширяем, поэтому шаблон покроет и типы, которых ещё нет.
- Не пользуйтесь cluster-admin в повседневной работе. Не добавляйте людей в system:masters: эта группа вообще пропускает проверки RBAC.
- list и watch на Secrets раскрывают содержимое, не только get. Право создавать Pod позволяет смонтировать Secrets и ConfigMaps этого пространства и работать от любого ServiceAccount там.
- Создание PersistentVolume может означать hostPath и файловую систему узла. Создание PV оставляют доверенным операторам и provisioner управляющего контура. Пользователи берут диск через заявки.
- Глаголы escalate, bind и impersonate, плюс одобрение CSR и serviceaccounts/token, могут поднять привилегии. Привязки просматривают по расписанию.

**When to use:**

RoleBinding в пространстве имён — для команд приложений. Права на весь кластер — только у немногих контроллеров, которым видны все пространства.

**Sources:**

- https://kubernetes.io/docs/concepts/security/rbac-good-practices/
</details>

<details>
<summary>Опишите how to securely manage Secrets in Kubernetes.</summary>

### Опишите how to securely manage Secrets in Kubernetes.

**Definition:**

Secret хранит небольшое чувствительное значение — пароль, токен или ключ — вне образа и вне spec Pod. По умолчанию этот объект в etcd не зашифрован.

**How it works:**

- Secrets лежат в etcd без шифрования, пока не включено шифрование покоя. Кто читает API или etcd, читает и их. Кто может создать Pod в пространстве имён, может прочитать Secrets там, в том числе создав Deployment.
- Включите шифрование покоя, сузьте RBAC до минимума, ограничьте, какие контейнеры монтируют Secret, и рассмотрите внешнее хранилище секретов.
- Pod может взять Secret как переменную окружения, том (включая ключ-dotfile) или учётные данные для pull, чтобы kubelet тянул образ из закрытого реестра.
- Те же байты не кладут в ConfigMap. ConfigMap — для несекретных данных и не является механизмом секретности.
- list и watch по Secrets возвращают данные, поэтому эти глаголы так же чувствительны, как get.

**Sources:**

- https://kubernetes.io/docs/concepts/configuration/secret/
- https://kubernetes.io/docs/concepts/security/rbac-good-practices/
- https://kubernetes.io/docs/concepts/configuration/configmap/
</details>

<details>
<summary>Что такое security context, and how does it apply to pod security?</summary>

### Что такое security context, and how does it apply to pod security?

**Definition:**

Безопасность Pod — это ограничения, которые не дают нагрузке превратиться в доступ к узлу. Kubernetes добивается этого политикой admission и тем, что не выдаёт мощные ServiceAccount, а не только полем одного контейнера.

**How it works:**

- Право создавать Pod уже сильное: Pod может смонтировать Secrets и ConfigMaps и работать от любого ServiceAccount пространства. Привилегированный Pod может дотянуться до узла.
- Если вызывающей стороне нет полного доверия, включайте стандарт Pod Security Baseline или Restricted через Pod Security admission или равноценный контроль.
- Мощные Pod не ставят рядом с недоверенными. Taint, toleration, node affinity или pod anti-affinity их разводят. Привилегированных DaemonSet должно быть мало, и у них минимум прав.
- Ставьте automountServiceAccountToken: false, если Pod не должен звать API.
- Граница аренды — пространство имён. Права внутри одного пространства — слабая стена.

**When to use:**

Restricted — для нагрузок, открытых в сеть. Baseline — когда образу всё же нужны несколько проверенных возможностей хоста. Privileged — только компонентам, которым обязательно касаться узла.

**Sources:**

- https://kubernetes.io/docs/concepts/security/rbac-good-practices/
</details>

<details>
<summary>Как NetworkPolicies contribute to pod security, and how would you implement them?</summary>

### Как NetworkPolicies contribute to pod security, and how would you implement them?

**Definition:**

NetworkPolicy ограничивает, какие соединения Pod может принять или открыть, на уровне IP и порта, для TCP, UDP и SCTP. Без сетевого плагина, который её исполняет, она ничего не делает.

**How it works:**

- По умолчанию Pod не изолирован: разрешены весь входящий и весь исходящий трафик. Политика, которая выбирает Pod и указывает Ingress или Egress в policyTypes, изолирует это направление.
- После изоляции разрешены только соединения из объединения подходящих политик, плюс ответный трафик и трафик к узлу и от узла. Pod не может заблокировать сам себя.
- Собеседники — другие Pod, пространства имён или блоки CIDR, их выбирают селекторами. Правило egress источника и правило ingress назначения оба должны разрешить соединение.
- Политики складываются. Порядок не важен.
- В пространстве имён ставят запрет по умолчанию, затем разрешают конкретные порты приложения. Объект политики без плагина, который её применяет, пуст.

**When to use:**

NetworkPolicy — для ограничений восток-запад внутри кластера. Это не замена RBAC, и она не видит трафик, который не касается Pod.

**Sources:**

- https://kubernetes.io/docs/concepts/services-networking/network-policies/
</details>

### Observability and Monitoring

<details>
<summary>Опишите the role and CAPabilities of the Metrics Server in a Kubernetes cluster.</summary>

### Опишите the role and CAPabilities of the Metrics Server in a Kubernetes cluster.

**Definition:**

Metrics Server — дополнение кластера, которое отдаёт текущие CPU и память, чтобы контроллеры автомасштабирования видели утилизацию. Это не часть ядра управляющего контура.

**How it works:**

- HorizontalPodAutoscaler меняет число реплик по наблюдаемой утилизации ресурсов, например CPU или памяти. Эти числа должны приходить из конвейера метрик.
- VerticalPodAutoscaler, сам по себе дополнение, требует установленный Metrics Server.
- Обзор компонентов называет мониторинг ресурсов контейнеров дополнением: он собирает и хранит метрики контейнеров отдельно от etcd и API-сервера.
- Он отвечает на вопрос «сколько Pod потребляет сейчас». Это не долгое хранилище мониторинга.

**When to use:**

Ставьте его, когда HPA или VPA должны масштабироваться по CPU или памяти. Отдельную систему мониторинга оставляют для истории и оповещений.

**Sources:**

- https://kubernetes.io/docs/concepts/workloads/autoscaling/
- https://kubernetes.io/docs/concepts/overview/components/
</details>

<details>
<summary>Как Prometheus be configured to monitor Kubernetes components?</summary>

### Как Prometheus be configured to monitor Kubernetes components?

**Definition:**

Prometheus не является компонентом Kubernetes. Кластер ждёт дополнение, которое собирает метрики контейнеров; Prometheus — одна из систем, которые команды ставят рядом с API-сервером для этой работы.

**How it works:**

- Ядро: API-сервер, etcd, планировщик, менеджер контроллеров, kubelet и среда выполнения. Их здоровье — это управляющий контур. Сбор метрик в обзоре компонентов указан отдельно как дополнение.
- Скрейпер направляют на компоненты, которые уже работают, и не дают ему cluster-admin. Практика RBAC — минимум прав и никакого шаблонного чтения.
- Автомасштабированию всё равно нужен короткий путь метрик ресурсов, например Metrics Server, для CPU и памяти. Дополнение мониторинга — место для истории.
- Дашборды не кладут в тот же etcd вместо дополнения метрик. etcd хранит объекты API.

**When to use:**

Берите его, когда нужен временной ряд метрик кластера и нагрузок. Metrics Server — когда единственный потребитель это HPA или VPA.

**Sources:**

- https://kubernetes.io/docs/concepts/overview/components/
- https://kubernetes.io/docs/concepts/workloads/autoscaling/
- https://kubernetes.io/docs/concepts/security/rbac-good-practices/
</details>

<details>
<summary>Обсудите the integration of Grafana for Kubernetes observability.</summary>

### Обсудите the integration of Grafana for Kubernetes observability.

**Definition:**

Grafana не входит в Kubernetes. Наблюдаемость в кластере — забота дополнений: метрики ресурсов и журналы уровня кластера, которые читает система вне управляющего контура.

**How it works:**

- Страница компонентов называет два подходящих дополнения: мониторинг ресурсов контейнеров и журналы уровня кластера, которые складывают логи контейнеров в центральное хранилище.
- Дашборд читает эти хранилища. Он не заменяет API-сервер, и ему не нужны права cluster-admin только чтобы рисовать графики.
- HPA по-прежнему масштабируется из конвейера метрик ресурсов, а не из запроса дашборда.
- Учётные данные дашборда хранят как любой Secret: шифрование покоя, RBAC с минимумом прав и никакой копии в ConfigMap.

**When to use:**

Дашборд нужен, когда люди смотрят те же ряды, которые дополнение уже хранит. Он не нужен, если единственное решение — число реплик HPA.

**Sources:**

- https://kubernetes.io/docs/concepts/overview/components/
- https://kubernetes.io/docs/concepts/workloads/autoscaling/
- https://kubernetes.io/docs/concepts/configuration/secret/
</details>

### Kubernetes Scheduling and Scalability

<details>
<summary>Как you use Node Selectors and Affinity rules to influence pod placement?</summary>

### Как you use Node Selectors and Affinity rules to influence pod placement?

**Definition:**

nodeSelector и affinity говорят планировщику, на каких узлах может работать Pod. Планировщик и так раскладывает Pod по узлам со свободными ресурсами; эти поля добавляют ограничение.

**How it works:**

- nodeSelector — простая форма. Pod работает только на узлах, у которых есть каждая указанная метка.
- Affinity выразительнее. Обязательное правило должно совпасть. Предпочтительное всё равно позволяет планирование, если совпадения нет. Pod affinity и anti-affinity смотрят на метки других Pod, поэтому два сервиса можно посадить вместе или врозь.
- nodeName обходит планировщик и привязывает Pod к этому узлу. Осушенный узел всё равно запустит Pod с nodeName.
- Для меток изоляции берите префикс, который kubelet не может сменить. Admission-плагин NodeRestriction запрещает kubelet писать в node-restriction.kubernetes.io/.
- Назначение делает kube-scheduler, компонент управляющего контура.

**When to use:**

nodeSelector — для жёсткой метки вроде типа диска. Предпочтительный affinity — когда размещение это смещение. Anti-affinity — когда две копии не должны делить домен отказа.

**Sources:**

- https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/
- https://kubernetes.io/docs/concepts/overview/components/
- https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/
</details>

<details>
<summary>Объясните Horizontal Pod Autoscaling (HPA) and its triggers within Kubernetes.</summary>

### Объясните Horizontal Pod Autoscaling (HPA) and its triggers within Kubernetes.

**Definition:**

HorizontalPodAutoscaler — объект API Kubernetes и контроллер, который меняет число реплик нагрузки так, чтобы оно следовало за наблюдаемой утилизацией.

**How it works:**

- Он периодически подстраивает реплики объекта, который управляет Pod, например Deployment, по сигналам вроде использования CPU или памяти.
- Это горизонтальное масштабирование. Вертикальное меняет ресурсы существующих Pod и устроено иначе. VPA — дополнение и требует Metrics Server.
- Ручное масштабирование остаётся: меняют число реплик или патчат запросы ресурсов без контроллера.
- KEDA может масштабировать ту же идею от событий, например сообщений в очереди, или от расписания cron. Это не встроенные триггеры HPA.
- Если лишние Pod не помещаются, следующий шаг — больше узлов, а не большее число HPA, которое не планируется.

**When to use:**

HPA — когда число реплик должно следовать за CPU, памятью или другой метрикой утилизации. Скейлер по очереди — когда триггер это хвост, а не CPU.

**Sources:**

- https://kubernetes.io/docs/concepts/workloads/autoscaling/
</details>

### Kubernetes Troubleshooting and Debugging

<details>
<summary>Какие шаги вы предпримете для troubleshooting pod, который не запускается?</summary>

### Какие шаги вы предпримете для troubleshooting pod, который не запускается?

**Definition:**

Pod стартует, когда планировщик может его разместить, а kubelet — запустить контейнеры. Pod, который не становится Ready, застрял на одном из этих двух шагов.

**How it works:**

- Если он Pending, планировщик его не привязал. Сверьте запросы со свободными ресурсами узла, nodeSelector и affinity, taint и PVC, которая ещё не связана. Заявка с WaitForFirstConsumer ждёт, пока размещение возможно. Заявка больше любого PV будет ждать бесконечно.
- Если он назначен, но контейнеры не идут, сбой у kubelet или среды выполнения: образ не скачивается, либо нет Secret или ConfigMap, на который есть ссылка. Pull из закрытого реестра требует Secret, которым может пользоваться kubelet.
- NetworkPolicy не мешает контейнеру стартовать. Она отбрасывает соединения уже после подъёма Pod, и только если плагин её применяет.
- kubectl drain здесь не инструмент. Drain выселяет уже работающие Pod, чтобы обслужить узел.
- Защита хранилища может оставить PVC в Terminating с finalizer kubernetes.io/pvc-protection, пока Pod её использует. Тогда новая заявка с тем же именем не свяжется.

**Sources:**

- https://kubernetes.io/docs/concepts/overview/components/
- https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/
- https://kubernetes.io/docs/concepts/storage/persistent-volumes/
- https://kubernetes.io/docs/concepts/configuration/secret/
- https://kubernetes.io/docs/concepts/services-networking/network-policies/
</details>

<details>
<summary>Как бы вы diagnose network issues in Kubernetes?</summary>

### Как бы вы diagnose network issues in Kubernetes?

**Definition:**

Сеть Kubernetes — это правила Service у kube-proxy плюс то, что делает сетевой плагин кластера. NetworkPolicy влияет на трафик только если плагин её исполняет.

**How it works:**

- kube-proxy на узле держит правила, которые реализуют Service. Если он не работает, первыми под подозрением VIP Service. Трафик по IP Pod — дело плагина, не kube-proxy.
- По умолчанию Pod принимает и открывает любые соединения. После изоляции направления действует только объединение подходящих правил, и оба конца должны разрешить поток.
- Объект политики в кластере, чей плагин не применяет NetworkPolicy, ничего не меняет. Это похоже на ошибку политики, а на деле нет контроллера.
- Трафик к узлу, где работает Pod, и от него всегда разрешён, независимо от IP Pod. Pod не может заблокировать сам себя.
- DNS — дополнение, не компонент управляющего контура. Имена не резолвятся, а IP работает — смотрите это дополнение, не API-сервер.

**Sources:**

- https://kubernetes.io/docs/concepts/services-networking/network-policies/
- https://kubernetes.io/docs/concepts/overview/components/
</details>

### Advanced Kubernetes Features

<details>
<summary>Определите Custom Resource Definitions (CRDs) and their utility in Kubernetes.</summary>

### Определите Custom Resource Definitions (CRDs) and their utility in Kubernetes.

**Definition:**

CustomResourceDefinition добавляет тип ресурса в API-сервер. После создания CRD у API появляется REST-путь для каждой перечисленной версии.

**How it works:**

- Имя CRD — это plural.group. spec.group и spec.versions задают путь /apis/<group>/<version>. spec.scope — namespaced или cluster-scoped.
- Удаление пространства имён удаляет пользовательские объекты в нём. Сам объект CRD не принадлежит пространству имён.
- У каждой версии есть флаг served. API-сервер отдаёт только версии, которые вы отметили served.
- Контроллеры и дополнения используют CRD для поведения, которого нет у встроенных Pod или Service. Документация автомасштабирования отмечает, что VPA ставится как custom resource, а не как тип ядра API.
- На текущей странице задачи CRD доступны с Kubernetes 1.16. Проверьте kubectl version, прежде чем применять манифест под более новый API.

**When to use:**

CRD добавляют, когда контроллеру нужно хранить свои объекты в API. ConfigMap — когда Pod нужны только пары ключ-значение.

**Sources:**

- https://kubernetes.io/docs/tasks/extend-kubernetes/custom-resources/custom-resource-definitions/
- https://kubernetes.io/docs/concepts/workloads/autoscaling/
</details>

<details>
<summary>Обсудите the functionalities provided by a service mesh like Istio in Kubernetes.</summary>

### Обсудите the functionalities provided by a service mesh like Istio in Kubernetes.

**Definition:**

Сервисная сеть не является компонентом управляющего контура Kubernetes. Kubernetes уже даёт Service, kube-proxy и NetworkPolicy; сеть — дополнительный слой, который команды ставят рядом.

**How it works:**

- kube-proxy реализует пересылку Service на узле. NetworkPolicy, если плагин её применяет, разрешает или запрещает соединения по Pod, пространству имён или CIDR.
- Эти два механизма не повторяют вызов, не перекладывают трафик между версиями и не добавляют взаимную идентичность соединения. Поэтому сеть ставят отдельным ПО.
- К сети применяют тот же RBAC, что и к любому мощному дополнению: никакого cluster-admin на повседневный трафик и никакого шаблонного чтения Secrets.
- Sidecar сети, который монтирует каждый Secret пространства, — тот же риск, который страница RBAC описывает для любого создателя Pod.
- Настройки сети без секретов кладут в ConfigMap, учётные данные — в Secret.

**When to use:**

Сеть добавляют, когда нужно поведение, которого нет в API Service и NetworkPolicy. Остаются на Service и NetworkPolicy, когда хватает списков доступа и виртуальных IP.

**Sources:**

- https://kubernetes.io/docs/concepts/overview/components/
- https://kubernetes.io/docs/concepts/services-networking/network-policies/
- https://kubernetes.io/docs/concepts/security/rbac-good-practices/
- https://kubernetes.io/docs/concepts/configuration/secret/
</details>

<details>
<summary>Что такое the use cases for Kubernetes Federation?</summary>

### Что такое the use cases for Kubernetes Federation?

**Definition:**

Федерация — способ направить несколько кластеров на одну задачу. У каждого кластера свой управляющий контур: свой API-сервер и свой etcd.

**How it works:**

- Планировщик одного кластера ставит Pod только на узлы этого кластера. Второй регион — это второй кластер, а не метка узла.
- Федерацию берут, когда приложение должно работать больше чем в одном таком кластере: второй домен отказа или пользователи близко к нескольким регионам.
- Node affinity и topology spread двигают Pod только внутри одного кластера. Они не заменяют второй управляющий контур.
- RBAC, Secrets и NetworkPolicy не переходят между кластерами. Каждому кластеру нужны свои привязки с минимумом прав и своё шифрование Secrets в покое.
- Добавление узлов масштабирует один кластер. Федерация — шаг после того, как домена отказа одного кластера мало.

**When to use:**

Несколько кластеров — когда недопустим простой одного управляющего контура или одного региона. Один кластер с topology spread — когда копиям нужны только разные узлы.

**Sources:**

- https://kubernetes.io/docs/concepts/overview/components/
- https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/
- https://kubernetes.io/docs/concepts/security/rbac-good-practices/
</details>

### Kubernetes Ecosystem and Tools

<details>
<summary>Как Helm enhance application management within Kubernetes?</summary>

### Как Helm enhance application management within Kubernetes?

**Definition:**

Helm не является API Kubernetes. Kubernetes применяет объекты вроде Deployment, Service, ConfigMap и CRD; Helm — клиент, который собирает эти объекты из пакета и отправляет их.

**How it works:**

- Чарт — набор манифестов с параметрами под одно окружение. Тот же чарт может задать другой ConfigMap или число реплик в другом кластере.
- API-сервер всё равно хранит получившиеся объекты. etcd не знает, что чарт существовал.
- CRD, которые ставит чарт, подчиняются правилам CRD: определение на уровне кластера, а имена пользовательских объектов умирают вместе с пространством имён.
- Secrets из чарта по-прежнему не зашифрованы в etcd, пока не включено шифрование покоя, и им всё ещё нужен RBAC с минимумом прав.
- Чарт удобен, когда много объектов ставят вместе. Один манифест — когда выпуск это один Deployment.

**Sources:**

- https://kubernetes.io/docs/concepts/overview/components/
- https://kubernetes.io/docs/tasks/extend-kubernetes/custom-resources/custom-resource-definitions/
- https://kubernetes.io/docs/concepts/configuration/secret/
- https://kubernetes.io/docs/concepts/configuration/configmap/
</details>

<details>
<summary>Объясните the role of Kustomize in Kubernetes configuration management.</summary>

### Объясните the role of Kustomize in Kubernetes configuration management.

**Definition:**

Kustomize накладывает патчи на обычные манифесты Kubernetes под окружение и не превращает их в язык шаблонов. API-сервер по-прежнему получает обычные объекты.

**How it works:**

- Базовые манифесты держат Deployment, Service, ConfigMap или Secret. Overlay меняет имена, метки или несколько полей для одного окружения.
- Данные ConfigMap не секретны и ограничены 1 MiB. Секретные значения остаются в Secrets, с шифрованием покоя.
- ConfigMap, смонтированный томом, kubelet со временем обновляет. Те же данные в переменных окружения не обновляются; Pod нужно перезапустить. Монтирование через subPath тоже не видит обновлений.
- immutable: true у ConfigMap или Secret запрещает менять data. Объект удаляют и создают заново. Это снижает нагрузку watch на apiserver, когда тысячи Pod их монтируют.
- Pod и ConfigMap должны быть в одном пространстве имён, если только приложение само не читает API.

**When to use:**

Overlay — когда окружения отличаются небольшим патчем. Чарт — когда нужна упакованная установка с записанным выпуском.

**Sources:**

- https://kubernetes.io/docs/concepts/configuration/configmap/
- https://kubernetes.io/docs/concepts/configuration/secret/
</details>

<details>
<summary>Опишите the importance of a container Registry in a Kubernetes environment.</summary>

### Опишите the importance of a container Registry in a Kubernetes environment.

**Definition:**

Среда выполнения контейнеров на каждом узле скачивает образы и запускает их. Реестр — место, где образы лежат; kubelet их скачивает, в том числе из закрытого реестра с помощью Secret.

**How it works:**

- Компоненты узла: kubelet, необязательный kube-proxy и среда выполнения контейнеров. Без среды выполнения образ не из чего запускать.
- Secret может хранить учётные данные реестра, чтобы kubelet скачал закрытый образ. Этот Secret чувствителен: нужны шифрование покоя и RBAC с минимумом прав.
- Кто может создать Pod в пространстве имён, может пользоваться Secrets там, поэтому Secret закрытого реестра этому человеку доступен.
- Выбор образа отдельно от планирования. nodeSelector может требовать узел с локальным диском или меткой, но образ он не хранит.
- Пароли не зашивают в образ. Secret для того и есть, чтобы секретные данные не попадали в образ и в spec Pod.

**Sources:**

- https://kubernetes.io/docs/concepts/overview/components/
- https://kubernetes.io/docs/concepts/configuration/secret/
- https://kubernetes.io/docs/concepts/security/rbac-good-practices/
</details>

### Best Practices in Kubernetes

<details>
<summary>Какие best practices вы следуете для high availability в Kubernetes deployments?</summary>

### Какие best practices вы следуете для high availability в Kubernetes deployments?

**Definition:**

Высокая доступность значит, что управляющий контур продолжает отвечать, а у нагрузки остаётся кворум готовых Pod. etcd — согласованное хранилище, от которого зависят эти решения.

**How it works:**

- Управляющий контур держат так, чтобы API-сервер и etcd оставались доступны. Страница компонентов называет etcd согласованным и высокодоступным хранилищем всех данных API. Один etcd — одно место, где эти данные могут остановиться.
- Реплики разносят anti-affinity или ограничениями топологии, чтобы потеря одного узла не была потерей всех реплик. Планировщик и так обходит узлы без свободных ресурсов.
- PodDisruptionBudget ставят до того, как кто-то осушит узел. Drain не выселит ниже minAvailable.
- HPA добавляет реплики, когда растёт нагрузка. Он не заменяет исчезнувший узел; для этого нужна ёмкость в другом месте.
- Secrets шифруют в покое и сужают RBAC, чтобы запасной узел управляющего контура не был ещё и запасной копией всех паролей открытым текстом.

**Sources:**

- https://kubernetes.io/docs/concepts/overview/components/
- https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/
- https://kubernetes.io/docs/concepts/workloads/autoscaling/
- https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/
- https://kubernetes.io/docs/concepts/configuration/secret/
</details>

<details>
<summary>Как ConfigMaps and Secrets improve configuration management?</summary>

### Как ConfigMaps and Secrets improve configuration management?

**Definition:**

ConfigMap хранит несекретные настройки в парах ключ-значение. Secret хранит небольшие секретные значения. Оба позволяют менять конфигурацию без пересборки образа.

**How it works:**

- У ConfigMap есть data для строк UTF-8 и binaryData для base64. Ключи не должны пересекаться. Объект не больше 1 MiB. Он не зашифрован, и пароль туда класть нельзя.
- Pod в том же пространстве имён может взять ConfigMap как аргументы контейнера, переменные окружения или файлы на томе только для чтения. Тома со временем обновляются; переменные окружения нет, и subPath тоже нет.
- Secret — секретный аналог: пароли, токены, ключи и учётные данные закрытого реестра. Включите шифрование покоя, иначе etcd хранит их открыто.
- immutable: true замораживает data. Это убирает случайные правки и снижает watch на apiserver, когда объект монтируют много Pod.
- spec статического Pod не может ссылаться на ConfigMap или другие объекты API.

**When to use:**

ConfigMap — для хостов, флагов и файлов, которые может читать всё пространство имён. Secret — для всего, что не должно всплывать при разборе манифеста.

**Examples:**

```yaml
# ConfigMap
data:
  DATABASE_HOST: db.internal
# Secret — секретно, шифруйте etcd в покое
```

**Sources:**

- https://kubernetes.io/docs/concepts/configuration/configmap/
- https://kubernetes.io/docs/concepts/configuration/secret/
</details>

<details>
<summary>Опишите multitenancy considerations for resource utilization in Kubernetes.</summary>

### Опишите multitenancy considerations for resource utilization in Kubernetes.

**Definition:**

Пространства имён — граница Kubernetes между арендаторами. Для всего, что Pod внутри пространства может смонтировать, это слабая стена, поэтому сверху нужны лимиты потребления и политика admission.

**How it works:**

- Разные уровни доверия разносят по разным пространствам имён. Права внутри одного пространства считают общими: создатель Pod может читать Secrets и ConfigMaps и работать от любого ServiceAccount этого пространства.
- Resource quota ограничивает, сколько объектов арендатор может создать. Страница RBAC отмечает, что безудержное создание объектов может исчерпать etcd; квоты — названное там смягчение.
- Арендаторам не дают создавать PersistentVolume. hostPath PV — это доступ к диску узла. Арендаторы просят хранилище заявками.
- NetworkPolicy действует в пространстве имён и складывается, но только если плагин её применяет. Без этого арендаторы делят открытую сеть.
- HPA и requests не дают нагрузке одного арендатора занять все свободные ядра только если requests заданы. Pod без requests легко поставить и легко им задушить соседей.
- Метки пространств ставят осторожно. Страница RBAC предупреждает, что пользователь, который может патчить метки пространства, может ослабить Pod Security или выбор NetworkPolicy.

**Sources:**

- https://kubernetes.io/docs/concepts/security/rbac-good-practices/
- https://kubernetes.io/docs/concepts/services-networking/network-policies/
- https://kubernetes.io/docs/concepts/workloads/autoscaling/
- https://kubernetes.io/docs/concepts/storage/persistent-volumes/
</details>
