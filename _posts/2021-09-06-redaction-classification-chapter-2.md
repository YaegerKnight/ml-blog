---
toc: true
layout: post
description: Explanation of how I trained a model to detect redactions in FOIA requests.
categories: [fastai, redactionmodel, computervision, datalabelling]
comments: true
author: Alex Strick van Linschoten
title: Training a classifier to detect redacted documents with fastai
---
I am working my way through [the fastai course](https://course.fast.ai) as part of an [online meetup group](https://www.meetup.com/delft-fast-ai-study-group) I host.[^1]

This week we finished the first and second chapters of the book, during which you train a model that can recognise if an image contains a cat or a dog. Later on, you train another model that distinguishes between different types of bears ('grizzly', 'black' and 'teddy').

[Jeremy Howard](https://twitter.com/jeremyphoward), who is teaching the course, then prompts you to take what you learned and apply it to something that has meaning for you. (This is something that most of those who've found any success with the course [emphasise repeatedly](https://sanyambhutani.com/how-not-to-do-fast-ai--or-any-ml-mooc-/).)

I decided to work on something adjacent to my previous life / work, where I knew there was some real-world value to be gained from such a model. I chose to train an image classifier model which would classify whether a particular image was redacted or not.

## The Problem Domain: Image Redaction

Under the [Freedom of Information Act](https://en.wikipedia.org/wiki/Freedom_of_Information_Act_(United_States)) (FOIA), individuals can request records and information from the US government.[^2] This is [one collection](https://www.esd.whs.mil/FOIA/Reading-Room/Reading-Room-List_2/) of some of the responses to this requests, sorted into various categories. You can read, for example, responses relating to UFOs and alien visits [here](https://www.esd.whs.mil/FOIA/Reading-Room/Reading-Room-List/UFO/).

Quite often, however, these images are censored or redacted.

![]({{ site.baseurl }}/images/redacted_sample.png "A name and an email address have been redacted here")

Knowing that this practice exists, I thought it might be interesting to train a model that could recognise whether a particular page contained some kind of redaction. This wasn't completely in line with what we covered during the first two chapters; I wasn't sure if the pre-trained model we used would work for this data set and use case.

It could be useful to have such a tool, because FOIA responses can sometimes contain lots of data. In order to prepare a request for more data, you might want to be able to show that even though you were sent thousands of pages, most of those pages contained redactions and so were effectively useless.

In the ideal vision of this tool and how it would work, you could run a programme out of a particular directory and it would tell you how many pages (and what proportion) of your PDF files were redacted.

## Getting the Data

The first thing I did to gather my data was to download the PDF documents available on [this site](https://www.esd.whs.mil/FOIA/Reading-Room/Reading-Room-List_2/). I knew that they contained examples of redactions in FOIA documents. I used [Automator](https://support.apple.com/en-gb/guide/automator/welcome/mac) to split the PDF files up into individual images.[^3] My Automator script did some downsampling of the images as part of the process, so the images were resized to something that wasn't prohibitively large to use for training.

Note that this stage and the next was done on my local machine. A CPU was enough for my purposes at this point, though probably I'll want to eventually port the entire process over to a single cloud machine to handle things end-to-end.

At the end of the splitting-and-resizing process, I had a little over 67,000 images (of individual pages) to train with.

## Labelling data with Prodigy

I had used Explosion.ai's [Prodigy data labelling tool](https://prodi.gy) in the past and so already had a license. The UI is clean and everything works pretty much as you'd hope. I had some teething issues getting it all working, but [Ines helped me work through those queries](https://support.prodi.gy/t/labelling-a-set-of-images-classification/4608/1) and I was up and running pretty quickly.

![]({{ site.baseurl }}/images/prodigy-interface.png "The interface for image classification looked like this")

It took about three hours to annotate some 4600+ images. Then I could export a `.jsonl` file that contained the individual annotations for whether a particular image contained a redaction or not:

![]({{ site.baseurl }}/images/annotations_jsonl.png)

From that point it was pretty trivial to parse the file (using [`json-lines` package](https://pypi.org/project/json-lines/)), and to resize the images down further in order to separate redacted from unredacted:

```python
import json_lines
from PIL import Image
from pathlib import Path

def save_resized_image_file(location_path):
    basewidth = 800
    img = Image.open(record['image'])
    wpercent = (basewidth / float(img.size[0]))
    hsize = int((float(img.size[1]) * float(wpercent)))
    img = img.resize((basewidth, hsize), Image.ANTIALIAS)
    img.save(location_path)

path = '/my_projects_directory/redaction-model'

redacted_path = path + "/redaction_training_data/" + "redacted"
unredacted_path = path + "/redaction_training_data/" + "unredacted"

with open(path + "/" + "annotations.jsonl", "rb") as f: # opening file in binary(rb) mode    
    for record in json_lines.reader(f):
        if record["answer"] == "accept":
            save_resized_image_file(Path(redacted_path + "/" + record['meta']['file']))
        else:
            save_resized_image_file(Path(unredacted_path + "/" + record['meta']['file']))
```

## Transferring the data to Paperspace with `magic-wormhole`

Once I had the two directories filled with the two sets of images, I zipped them up since I knew I'd want to use them on a GPU-enabled computer.

I used [`magic-wormhole`](https://magic-wormhole.readthedocs.io) to transfer the files over to my [Paperspace Gradient](https://gradient.paperspace.com) machine. The files were only about 400MB in size so it took less than a minute to transfer the data.

Again, ideally I wouldn't have this step of doing things locally first. I could certainly have done everything on the Paperspace machine from the very start, but it would have taken a bit of extra time to figure out how to process the data programatically.

## Using the labelled data in our training

## Initial results

## Experimenting with augmentations

## Experimenting with different architectures

## Hosting the model with MyBinder

## Possible next steps

- more labelled data?
- simple binary classification?
- different types of redaction (handwritten vs computer) and a way to combine?
- segmentation model?

## Footnotes

[^1]: You can find our thread in the fastai forum [here](https://forums.fast.ai/t/virtual-study-group-delft-the-netherlands-europe-time-zone/90521).
[^2]: Other countries have variations of this law, like [this](https://www.gov.uk/make-a-freedom-of-information-request) from the United Kingdom.
[^3]: I realise that there is a programatic way to do this. At this early stage in the project, I was more eager to get going with the labelling, so I took the easy path by using Automator.