---
toc: false
layout: post
description: "A short post on setting environment variables using Python."
categories: [python]
comments: true
author: Alex Strick van Linschoten
title: How to set and get environment variables using Python
---

If you want to get and set environment variables using Python, simply use the relevant methods from `os`. To set an environment variable, do this:

```python
import os

os.environ['SOME_ENV_VARIABLE'] = 13.5
```

And to access an environment variable, there are actually a number of different ways. All these three are essentially the same:

```python
os.getenv('SOME_ENV_VARIABLE')
os.environ.get('SOME_ENV_VARIABLE')
os.environ('SOME_ENV_VARIABLE')
```

For the final one (`os.environ('SOME_ENV_VARIABLE')`), if the variable doesn't exist, it'll return a `KeyError`, whereas the first two will just return `None` in that case.