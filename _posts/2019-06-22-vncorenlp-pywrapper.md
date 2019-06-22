---
layout: post
title: "How I wrote a python wrapper for Java"
author: "Tran Ngoc Quy"
categories: journal
tags: [ml,nlp,python,java,programming]
image: posts/cutting.jpg
---

When working on my thesis, I have to experiment on different tokenize tool. One of those is [VnCoreNLP](https://github.com/vncorenlp/VnCoreNLP) which is reported to have very high accuracy. The thing is it's written in Java while my pipeline is written in Python. So, I decided to write a python wrapper for it. Let's go.

FYI: If you don't know it yet, the problem of calling others programming language is interoperability. For example, Java can call native function written in C/C++ using JNI.

## The problem

As I stated before, it's the matter of how to call Java implementation from python. After some consideration, I choose py4j for it simple usage.

## Preparation

Okay, as a rule of thumb, I always create a create a new virtual environment for any python project.

Let say we are in a new folder named `tokenizer`. To create a virtual environment, standing in `tokenzier`, you should type in the terminal like this.

```bash
python3 -m venv .env
```

Remember to activate it using

```bash
source .env/bin/activate
```

As always, install a python package is a piece of cake.

`pip install py4j`

And yes, because I'm writing a wrapper for VnCoreNLP, I can't miss it. Clone the [VnCoreNLP](https://github.com/vncorenlp/VnCoreNLP) repo. Placed VnCoreNLP-1.1.jar and the models directory of VnCoreNLP in the same working folder.

To write a python library you should create a folder name `__init__`

The working directory structure should look like this now.

```bash
tokenizer/
├── .env                        # Virtual environment folder
├── __init__                    # Python library files
├── models                      # VnCoreNLP models
└── VnCoreNLP-1.1.jar           # VnCoreNLP java lib
```

## Getting started

Create a new python file in the folder `__init__`, in my case I named it `vncorenlp.py`

In `vncorenlp.py` import `JavaGateway`

```python
from py4j.java_gateway import JavaGateway
```

It's the standard library that you must import if you want to call a Java instance.

Now, let's construct a class for the wrapper. I won't get into details, there will be full source code at the end of this post.

The most important of this wrapper is how to get the Java instance to run. Creating new instance of a Java class is easy using py4j, they didn't told about this, but I guess it's a thing to figure out.

```python
gateway = JavaGateway.launch_gateway(classpath=self.path, die_on_exit=True)
```

You shold pass a path to the jar file that encapsulating the Java function to the `launch_gateway` method. In this case, I'm passing the path through the class constructor so you see I get the path through `self` instance.

VnCoreNLP require an array of string containing anotators. What to note here is how to create a string list using py4j.

```python
annotators = gateway.new_array(gateway.jvm.java.lang.String, 1)
```

The `new_array` function receive first argument as the type of the array, for the `String` type, you can obtain it from `java.lang` library. The length of the array is specified in the following arguments. Here I create a 1 element array.

Assigning element to array is easy as it heard.

```python
annotators[0] = 'wseg'
```

Calling a Java class need fully qualified name like below.

```python
pipeline = gateway.jvm.vn.pipeline.VnCoreNLP(annotators)
annotation = gateway.jvm.vn.pipeline.Annotation(sentence)
```

After getting the Java class, calling a function is just like simple python function call.

```python
pipeline.annotate(annotation)
```

And that's all you need to note on how to write the wrapper.

The full project can be found [here](https://github.com/motmaytinh/vncorenlp-pywrapper)