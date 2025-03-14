---
title: Контейнеры
weight: 40
description: Технология упаковки приложения вместе с его runtime-зависимостями.
reviewers:
content_type: concept
no_list: true
---

<!-- overview -->

Каждый запускаемый контейнер воспроизводим; стандартизация благодаря включению зависимостей позволяет каждый раз получать одинаковое поведение при запуске.

Контейнеры абстрагируют приложения от базовой инфраструктуры хоста, упрощая развертывание в различных облачных средах или ОС.




<!-- body -->

## Образы контейнеров
[Образ контейнера](/docs/concepts/containers/images/) – это готовый к запуску пакет программного обеспечения, содержащий все необходимое для запуска приложения: код, среду исполнения, прикладные и системные библиотеки, а также значения по умолчанию всех важных параметров.

Контейнер по определению неизменяем (immutable): код работающего контейнера невозможно поменять. Чтобы внести правки в контейнеризованное приложение, необходимо собрать новый образ, содержащий эти правки, а затем запустить контейнер на базе обновленного образа.

## Исполняемые среды контейнеров

{{< glossary_definition term_id="container-runtime" length="all" >}}

## {{% heading "whatsnext" %}}

* Раздел об [образах контейнеров](/docs/concepts/containers/images/).
* Раздел о [Подах](/ru/docs/concepts/workloads/pods/).
