---
layout: post
title: "How I wrote a python wrapper for Java"
author: "Tran Ngoc Quy"
categories: journal
tags: [ml,nlp,python,java,programming]
# image: cutting.jpg
---

When working on my thesis, I have to experiment on different tokenize tool. One of those is [VnCoreNLP](https://github.com/vncorenlp/VnCoreNLP) which is reported to have very high accuracy. The thing is it's written in Java while my pipeline is written in Python. So, I decided to write a python wrapper for it. Let's go.

FYI: If you don't know it yet, the problem of calling others programming language is interoperability. Java can call native function written in C/C++ using JNI.

# The problem

As I stated before, it's the matter of how to call Java implementation from python. After some consideration, I choose py4j for it simple usage.

# Get started

## Preparation

As always, install a python package is a piece of cake.

`pip install py4j`