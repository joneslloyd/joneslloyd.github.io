# Salmon lice: Ch-ch-ch-ch-changes 👨‍🎤

After corresponding with the domain expert (Tina) some more, it turns out that the real-life situation I'm supposed to be applying ML to has evolved a little, so the application of machine learning must accordingly too.

The original real-life problem to solve was "*Count the number of lice, if any, in a given cleanerfish's stomach contents*".

However, after running their experiment for a bit, it turns out that the cleanerfish aren't *that* great at eating lice..

So only about 30 of the 900 cleanerfishes' sampled stomach contents contained any lice.

This in turn means that having a machine learning algorithm count the lice won't have a massively useful real-life impact.

## However

There are other ways in which ML can help!

Tina sent over some of the stomach contents photos.

Here are three, as a sample:

### Contents: Lice, Feed pellets, Biofouling
![Stomach contents sample: Lice, Feed pellets, Biofouling](https://joneslloyd.github.io/images/stomach-contents-sample--lice+feed-pellets+biofouling.png){:width="500px"}*Credit: Tina Marie Weier Oldham*

### Contents: Jellyfish, Biofouling
![Stomach contents sample: Jellyfish, Biofouling](https://joneslloyd.github.io/images/stomach-contents-sample--jellyfish+biofouling.png){:width="500px"}*Credit: Tina Marie Weier Oldham*

### Contents: Empty
![Stomach contents sample: Empty](https://joneslloyd.github.io/images/stomach-contents-sample--empty.png){:width="500px"}*Credit: Tina Marie Weier Oldham*

Rather than simply assess "*No lice or X lice*", Tina asked if I could instead:

1) Spot the presence of a number of discrete categories:
  - Jellyfish
  - Lice
  - "Biofouling"
  - Feed pellets

2) Give some measure of stomach contents quantity / "Fullness", perhaps expressed as a percentage, ie, Percent coverage of the petri dish bottom.

## How?!

### Approach/architecture

Maybe this requires multiple neural networks?

Right now, I'm thinking that (much like [Gilbert Tanner's example](https://gilberttanner.com/blog/fastai-multi-label-image-classification){:target="_blank" rel="noopener"}) some sort of multi-label classification neural network could be best for labelling each dish as containing things like biofouling, jellyfish, feed pellets (etc).

The counting of lice is no longer an issue, as explained, but figuring out the percentage coverage is.

Perhaps this can be approached as an image segmentation problem? ie,

- The images are cropped to an exact, standard width and height in pixels
- Petri dishes are cropped to an exact, standard size
- Any "stuff" is a segment
- The size/area of that segment can be compared against the image size, and a percentage coverage calculated?

### Lack of *stuff*

I recall Jeremy saying (as an aside in Part 2, Lesson 10) that trying to capture the presence of "Nothing" or "Lack of stuff" as an image label is a hopeless pursuit, because the network is always looking *for* something in image categorisation, ie, "*Is this amount of furriness associated with the image being a rabbit?*", etc.

So that will be something that needs to be thought about too.

### What next

In the mean-time:

- I'm going to continue watching part two of the Fast AI course and practising with my own code/examples for the next couple of weeks
- Tina is continuing with some of her own (unrelated) research for about the same period of time
- The images of stomach contents will be given some sort of early labelling via entry into a spreadsheet – Something like this: ![Stomach contents sample data table](https://joneslloyd.github.io/images/stomach-contents-mock-table.png){:width="500px"} This will help with multi-label classification, and testing whether the percentage coverage estimations that a ML algorithm generates are accurate. (Ignore the lice count column).
