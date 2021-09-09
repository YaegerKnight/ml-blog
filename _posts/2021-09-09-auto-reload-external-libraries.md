---
toc: false
layout: post
description: A small bit of Jupyter notebook magic
categories: [jupyter]
comments: true
author: Alex Strick van Linschoten
title: How to set a Jupyter notebook to auto-reload external libraries
---
The code to insert somewhere into your Jupyter notebook is pretty simple:

```python
%load_ext autoreload
%autoreload 2
```

When you're working on an external library or piece of Python code outside the contents of your notebook, this snippet will make sure that the updated functions and constants will always be available in their most-recently edited state.