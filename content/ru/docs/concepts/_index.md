---
title: Концепции
main_menu: true
content_type: concept
weight: 40
---

<!-- overview -->

Раздел "Концепции" поможет вам узнать о частях системы Kubernetes и об абстракциях, которые Kubernetes использует для представления вашего {{< glossary_tooltip text="кластера" term_id="cluster" length="all" >}}, и помогает вам глубже понять, как работает Kubernetes.



<!-- body -->

## Краткий обзор

Чтобы работать с Kubernetes, вы используете *объекты API Kubernetes* для описания *желаемого состояния вашего кластера*: какие приложения или другие рабочие нагрузки вы хотите запустить, какие образы контейнеров они используют, количество реплик, какие сетевые и дисковые ресурсы вы хотите использовать и сделать доступными и многое другое. Вы устанавливаете желаемое состояние, создавая объекты с помощью API Kubernetes, обычно через интерфейс командной строки `kubectl`. Вы также можете напрямую использовать API Kubernetes для взаимодействия с кластером и установки или изменения желаемого состояния.

После того, как вы установили желаемое состояние, *Управляющий слой Kubernetes* (control plane) заставляет текущее состояние кластера соответствовать желаемому состоянию с помощью генератора событий жизненного цикла подов ([Pod Lifecycle Event Generator, PLEG](https://github.com/kubernetes/design-proposals-archive/blob/main/node/pod-lifecycle-event-generator.md)). Для этого Kubernetes автоматически выполняет множество задач, таких как запуск или перезапуск контейнеров, масштабирование количества реплик данного приложения и многое другое. Управляющий слой Kubernetes состоит из набора процессов, запущенных в вашем кластере:

* **Мастер Kubernetes** — это коллекция из трех процессов, которые выполняются на одном узле в вашем кластере, который обозначен как главный узел. Это процессы: [kube-apiserver](/docs/admin/kube-apiserver/), [kube-controller-manager](/docs/admin/kube-controller-manager/) и [kube-scheduler](/docs/admin/kube-scheduler/).
* Каждый отдельный неосновной узел в вашем кластере выполняет два процесса:
  * **[kubelet](/docs/admin/kubelet/)**, который взаимодействует с мастером Kubernetes.
  * **[kube-proxy](/docs/admin/kube-proxy/)**, сетевой прокси, который обрабатывает сетевые сервисы Kubernetes на каждом узле.

## Объекты Kubernetes

Kubernetes содержит ряд абстракций, которые представляют состояние вашей системы: развернутые контейнеризованные приложения и рабочие нагрузки, связанные с ними сетевые и дисковые ресурсы и другую информацию о том, что делает ваш кластер. Эти абстракции представлены объектами в API Kubernetes. См. [Понимание объектов Kubernetes](/docs/concepts/overview/working-with-objects/kubernetes-objects) для получения более подробной информации.

Основные объекты Kubernetes включают в себя:

* [Pod](/docs/concepts/workloads/pods/pod-overview/)
* [Service](/docs/concepts/services-networking/service/)
* [Том](/docs/concepts/storage/volumes/)
* [Namespace](/docs/concepts/overview/working-with-objects/namespaces/)

Kubernetes также содержит абстракции более высокого уровня, которые опираются на [Контроллеры](/docs/concepts/architecture/controller/) для создания базовых объектов и предоставляют дополнительные функциональные и удобные функции. Они включают:

* [Deployment](/docs/concepts/workloads/controllers/deployment/)
* [DaemonSet](/docs/concepts/workloads/controllers/daemonset/)
* [StatefulSet](/docs/concepts/workloads/controllers/statefulset/)
* [ReplicaSet](/docs/concepts/workloads/controllers/replicaset/)
* [Job](/docs/concepts/workloads/controllers/jobs-run-to-completion/)

## Управляющий слой Kubernetes

Различные части управляющего слоя Kubernetes (control plane), такие как мастер Kubernetes и процессы kubelet, определяют, как Kubernetes взаимодействует с кластером. Управляющий слой поддерживает запись всех объектов Kubernetes в системе и запускает непрерывные циклы управления для обработки состояния этих объектов. В любое время циклы управления управляющего слоя будут реагировать на изменения в кластере и работать, чтобы фактическое состояние всех объектов в системе соответствовало желаемому состоянию, которое вы указали.

Например, когда вы используете API Kubernetes для создания развертывания, вы предоставляете новое желаемое состояние для системы. Управляющий слой Kubernetes записывает создание этого объекта и выполняет ваши инструкции, запуская необходимые приложения и планируя их на узлы кластера, чтобы фактическое состояние кластера соответствовало желаемому состоянию.

### Мастер Kubernetes

Мастер Kubernetes отвечает за поддержание желаемого состояния для вашего кластера. Когда вы взаимодействуете с Kubernetes, например, используя интерфейс командной строки `kubectl`, вы работаете с мастером Kubernetes вашего кластера.

> Под "мастером" понимается совокупность процессов, которые управляют состоянием кластера. Обычно все эти процессы выполняются на одном узле кластера, и поэтому этот узел называется главным (master). Мастер также может быть реплицирован для доступности и резервирования.

### Узлы Kubernetes

Узлы в кластере - это машины (виртуальные машины, физические серверы и т.д.), на которых работают ваши приложения и облачные рабочие процессы. Мастер Kubernetes контролирует каждый узел; вы редко будете взаимодействовать с узлами напрямую.




## {{% heading "whatsnext" %}}


Если вы хотите описать концепт, обратитесь к странице
[Использование шаблонов страниц](/docs/home/contribute/page-templates/)
для получения информации о типе страницы и шаблоне концепции.


