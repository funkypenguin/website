---
title: kube-scheduler
id: kube-scheduler
date: 2018-04-12
full_link: /docs/reference/generated/kube-scheduler/
short_description: >
  Komponente auf der Control Plane, die neu erstellte Pods überwacht, denen kein Knoten zugewiesen ist. Sie wählt den Knoten aus, auf dem sie ausgeführt werden sollen.

aka:
tags:
- architecture
---
 Komponente auf der Control Plane, die neu erstellte Pods überwacht, denen kein Knoten zugewiesen ist. Sie wählt den Knoten aus, auf dem sie ausgeführt werden sollen.

<!--more-->

Zu den Faktoren, die bei Planungsentscheidungen berücksichtigt werden, zählen individuelle und kollektive Ressourcenanforderungen, Hardware- / Software- / Richtlinieneinschränkungen, Affinitäts- und Anti-Affinitätsspezifikationen, Datenlokalität, Interworkload-Interferenz und Deadlines.


