---
layout: post
title: "[TIL] Map.computeIfAbsent() Method"
author: "Tran Ngoc Quy"
categories: tech
tags: [java,programming]
image: vncorenlp-pywrapper.png
---

## TL;DR

The cumbersome boilerplate code of putting an element into a list which reside in a map entry of Java like this...

```java
Map<String, List<Integer>> foo = new HashMap<>();

List<Integer> bars = foo.get("bar");
if (bars == null) {
    bars = new LinkedList<>();
    foo.put("bar", bars);
}
bars.add(100);
```

... can now be reduced to a one-liner like this

```java
foo.computeIfAbsent("bar", key -> new LinkedList<>()).add(100);
```

## The problem
