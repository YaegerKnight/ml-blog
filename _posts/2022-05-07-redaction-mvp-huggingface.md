---
toc: true
layout: post
description: "I created a few deployed MVP demos showcasing models I'd created while participating in the fastai course, uploading them to the Huggingface Hub and using a Gradio Demo hosted on Huggingface Spaces."
categories: [fastai, computervision, redactionmodel, tools]
comments: true
author: Alex Strick van Linschoten
title: "A painless way to create an MVP demo using computer vision models"
image: images/redaction-mvp-huggingface/demo-screenshot.png
---

After the second class of the fastai course, we're encouraged to create mini-projects that result in models we can deploy online. Deployment is a huge field with its own complexities, of course, but having an option to get something out in the world that's visible and usable is extremely useful.

# Step by step, iteration by iteration

This week I chose to use my previous work on redacted images to leverage a dataset [I'd previously collected](https://mlops.systems/fastai/redactionmodel/computervision/datalabelling/2021/09/06/redaction-classification-chapter-2.html). I wanted to showcase something useful and interesting and I ended up slightly blocked as to what I was going to build. After discussing it with [the study group](https://www.meetup.com/delft-fast-ai-study-group/) briefly, I was reminded not to try to bite off too much: start small with the smallest possible next version of what you want, and then continue from there.

Since I already had a large dataset of redacted and unredacted images (extracted from PDF documents available online), I used this to train a classification model that could tell whether a page contained redactions or not.

With that model exported, it was then easy to get a simple [Gradio](https://gradio.app) app demo up and running, particularly with the suggestions in Tanishq Abraham's [really useful tutorial blogpost](https://tmabraham.github.io/blog/gradio_hf_spaces_tutorial).

It's an easy step to go from having a Gradio app deployed to then hosting that same demo as a Huggingface Space, so I then did that. You can [access the demo here](https://huggingface.co/spaces/strickvl/fastai_redaction_classifier) at [`strickvl/fastai_redaction_classifier`](https://huggingface.co/spaces/strickvl/fastai_redaction_classifier).

![]({{ site.baseurl }}/images/redaction-mvp-huggingface/classification-demo.png "A Gradio app [hosted on Huggingface Spaces](https://huggingface.co/spaces/strickvl/fastai_redaction_classifier): an image classifier that detects whether an image input contains a redaction or not.")

At this first stage I had the exported model itself uploaded inside the Spaces repository, but [this useful blog](https://huggingface.co/blog/fastai) by Omar Espejel showed how I could just upload my model directly to the Huggingface model hub. Instead of calling `learn.export('model.pkl')` and uploading the model file itself, I could just run the following code after authentication:

```python
from huggingface_hub import push_to_hub_fastai

repo_id = "MY_USERNAME/MY_LEARNER_NAME"

push_to_hub_fastai(learner=learn, repo_id=repo_id)
```

My model [lives here](https://huggingface.co/strickvl/redaction-classifier-fastai) on the Huggingface model hub and can be directly saved or just used via the hosted Inference API.

# Using the inference API for more flexibility

Buoyed on by Tanishq's blog and the workflow we'd seen in the lecture that week, I thought it might be worth running my inference requests through the HTTP API instead of letting Huggingface handle all that.

Thanks to a really simple and comprehensible [example](https://github.com/nuvic/predict_image) made by [@Nuvic](https://github.com/nuvic) I was quickly able to get something up and running. The forked source code is available [here](https://github.com/strickvl/predict_redaction_classification) and the main website where you can try out the tool is here: [https://strickvl.github.io/predict_redaction_classification/](https://strickvl.github.io/predict_redaction_classification/). If you [search for 'redacted document' images](https://duckduckgo.com/?q=redacted+document&t=osx&iax=images&ia=images) and save one of them do your local computer you can use those to try it out.

![]({{ site.baseurl }}/images/redaction-mvp-huggingface/gh-pages-demo.png "A demo using Github Pages to host a simple app showcasing the model inference.")

While the demo gives a sense of the model's capabilities, in reality you would probably not find it very helpful to use a web app that required you to feed a document's pages to it one by one. I started to think about a more complex application where you could upload a PDF and it would split the PDF for you and do all the inference behind the scenes.

# Building an MVP of a redaction detection application

I spent a brief half-hour considering deploying a simple [Flask](https://flask.palletsprojects.com/en/2.1.x/) web app hosted somewhere for free before realising I didn't even need to go that far to create a proof of concept that would have the required functionality. I returned back to Huggingface Spaces hoping that I'd be able to build everything out.

You can access the demo / MVP app that I created here:
[https://huggingface.co/spaces/strickvl/redaction-detector](https://huggingface.co/spaces/strickvl/redaction-detector)

![]({{ site.baseurl }}/images/redaction-mvp-huggingface/demo-screenshot.png "[An MVP app](https://huggingface.co/spaces/strickvl/redaction-detector) for detection, extraction and analysis of PDF documents that contain redactions.")

This MVP app runs two models to mimic the experience of what a final deployed version of the project might look like.

- The first model (a classification model trained with fastai, available on the Huggingface Hub [here](https://huggingface.co/strickvl/redaction-classifier-fastai) and testable as a standalone demo [here](https://huggingface.co/spaces/strickvl/fastai_redaction_classifier)), classifies and determines which pages of the PDF are redacted. I've written about how I trained this model [here](https://mlops.systems/fastai/redactionmodel/computervision/datalabelling/2021/09/06/redaction-classification-chapter-2.html).
- The second model (an object detection model trained using [IceVision](https://airctic.com/) (itself built partly on top of fastai)) detects which parts of the image are redacted. This is a model I've been working on for a while and I described my process in [a series of blog posts](https://mlops.systems/categories/#redactionmodel).

This MVP app does several things:

- it extracts any pages it considers to contain redactions and displays that subset as an [image carousel](https://gradio.app/docs/#o_carousel). It also displays some text alerting you to which specific pages were redacted.
- if you click the "Analyse and extract redacted images" checkbox, it will:
- pass the pages it considered redacted through the object detection model
- calculate what proportion of the total area of the image was redacted as well as what proportion of the actual content (i.e. excluding margins etc where there is no content)
- create a PDF that you can download that contains only the redacted images, with an overlay of the redactions that it was able to identify along with the confidence score for each item.

# Lessons learned

I was — and continue to be — surprised that the free Huggingface Spaces environment has no problem running all this fairly compute-intensive inference on their backend. (That said, if you try to upload a document containing dozens or hundreds of pages and you'll quickly hit up against the edge of what they allow.)

I became very familiar with the [Gradio interface docs](https://gradio.app/docs/) while creating this app and was impressed by how customisable the final application could be. You don't have as much freedom as a web application written from scratch, but you still *do* have a lot of freedom.

Given how much inference is going on behind the scenes, I'm surprised that it runs as fast as it does. For a document with 4 or 5 redacted pages, it takes around 10 seconds to do all the steps described above. 10 seconds is still far too long for a scenario where you wanted to run inference over millions of pages, but in that scenario you wouldn't be manually uploading them on a web app either.

It's extremely gratifying to have these kinds of tools available to use for free, and really exciting that you get to build out prototypes of this kind after just two weeks of study on the fastai course.