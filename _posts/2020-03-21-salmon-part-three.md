# Salmon lice: Multi-label classificaiton

As previously mentioned (I think?), I was going to approach this problem in two stages:

__1)__ Multi-label classification

__2)__ Instance segementation

This post details the progress with multi-label classification and things around it.

## fastai2

I'm using fastai2, and have enrolled in this year's (remote, courtesy of coronavirus) *Deep Learning Part I* course.

## Data
Tina provided me with images (as already shown in a previous post). These are all fairly standardised, comprising a petri dish containin the 'items of interest', and a ruler to the left with an image/instance number (which we don't need).


## Dataset

Tina also provided a Google Sheet containing fish IDs, and columns for the presence/absence of each type of label:
![The CSV file](https://joneslloyd.github.io/images/google-sheet-sample.png){:width="500px"}

From that I have created a .CSV file that looks like this:
![The CSV file](https://joneslloyd.github.io/images/csv-sample.png){:width="500px"}

It contains (as with the PASCAL dataset) all labels separated by a space. There are only two columns; one for filename and one for labels.

## Jupyter Notebook

I have the following code in my Jupyter Notebook, heavily adapted from [Zachary Mueller](https://github.com/muellerzr/){:target="_blank" rel="noopener"}'s code.

```
# Use Google Drive + Colab
from google.colab import drive
drive.mount('/content/gdrive')
root_dir = "/content/gdrive/My Drive/"
base_dir = root_dir + 'Colab Notebooks/salmon/lice-multi-label/'
```

```
!pip install fastai2
```

```
# Required Fast AI imports
from fastai2.data.all import *
from fastai2.vision.all import *
from fastai2.callback.all import *
```

```
# Get Data Frame
df = pd.read_csv(base_dir + 'data-labels.csv')
# Print it
df.head()
# df.values
```

```
# DataBlock method
batch_tfms = [*aug_transforms(flip_vert=False, max_lighting=0.15, max_zoom=1.05, max_warp=0.02, max_rotate = 10.0, size=512),Normalize.from_stats(*imagenet_stats)]
lice = DataBlock(blocks=(ImageBlock, MultiCategoryBlock),
                   get_x=ColReader(0, pref=f'{base_dir}images/', suff=''),
                   splitter=RandomSplitter(),
                   get_y=ColReader(1, label_delim=' '),
                   batch_tfms = batch_tfms)
```
I chose not to perform vertical flipping, and other extreme transforms to the images, because in my dataset there is a uniformity among the photos:
- They are all taken from above (I believe with some sort of rig)
- They all feature a petri dish to the right of the image, containing the contents that we care about
- They all have a ruler to the left with a number on (which we don't care about)
- They are all taken from approximately the same distance
- They all contain the same amount of perspective warping (that is, zero)

```
# DataLoaders
# Try setting batch size to 24. The default was almost causing memory to max out.
dls = lice.dataloaders(df, bs=24)
dls.show_batch(max_n=9, figsize=(12,9))
```
__Output from `dls.show_batch`:__
![show_batch](https://joneslloyd.github.io/images/show-batch-sample.png){:width="500px"}

I set the batch size to 24, which for the architecture I'm using (shown below) appears to be optimal.

I tried 64 and 32, but Colab kept running out of memory. 24 was the maximum amount I could get away with.

```
# Imports
from torchvision.models import resnet34
from fastai2.metrics import accuracy_multi
```

```
# Train!

#Set seeds
def set_seeds():
    random.seed(42)
    np.random.seed(12345)
    torch.manual_seed(1234)
    torch.backends.cudnn.deterministic = True
    torch.backends.cudnn.benchmark = False

set_seeds()

# CNN Learner
learn = cnn_learner(dls, resnet34, pretrained=True, metrics=[partial(accuracy_multi, thresh=0.6)], loss_func = BCEWithLogitsLossFlat())

# Add to Learner
#learn.loss_func = BCEWithLogitsLossFlat()

learn.summary()
```

I'm using `BCEWithLogitsLossFlat` as the loss function, because there are potentially multiple target values for each image (rather than just one label/class).

I'm using `resnet34`, because (for now) this has performed best in tests. I tried `resnet18` and this performed noticably worse (about 60% accuracy, from memory).

I tried `resnet50`, and the results were almost identical to `resnet34`, but took longer.

```
learn.lr_find()
```
__Output from `learn.lr_find()`:__
![lr_find](https://joneslloyd.github.io/images/lr-find-sample.png){:width="500px"}

```
# Set learning rate
lr = 1e-1
learn = learn.to_fp16()
```
I chose 1<sup>e-1</sup> here both because of Jeremy's advice in the 2019 course (which point on the graph to use), and because (from experimenting) it performed better than 1<sup>e-2</sup> and 1<sup>e-3</sup>.

```
learn.fit_one_cycle(5, slice(lr))
```
__Output from `learn.fit_one_cycle(5, slice(lr))`:__
![fit_one_cycle](https://joneslloyd.github.io/images/lr-initial-fit-one-cycle.png){:width="500px"}

```
learn.save(base_dir + 'second-attempt-1')
```

```
learn.load(base_dir + 'second-attempt-1')
```

```
learn.unfreeze()
learn.lr_find()
```
__Output from unfrozen `learn.lr_find`:__
![learn.lr_find](https://joneslloyd.github.io/images/unfrozen-lr-find-sample.png){:width="500px"}

```
learn.fit_one_cycle(9, slice(1e-5, lr/5))
learn.save(base_dir + 'second-attempt-1-unfrozen')
```
__Output from unfrozen `learn.fit_one_cycle`:__
![learn.fit_one_cycle](https://joneslloyd.github.io/images/unfrozen-lr-fit-one-cycle.png){:width="500px"}

```
learn.export(base_dir + 'second-attempt-1-unfrozen.pkl')
```

## Inference

While the above all **looks** good, when I use this model for inference, the results are almost always badly wrong.

```
#Load learner
learn = load_learner(base_dir + 'second-attempt-1-unfrozen.pkl')

#Grab image
img = PILImage.create(base_dir + 'inference-test/biofouling-anemones.JPG')

#Get prediction for image
learn.predict(img)
```

Here's an example where the test image should have the label `'biofouling'`, but returns `'biofouling','biofouling-plus-pink','lice-females','lice-males','lice-preadult'`.

## Improving

My hunch is that there are too many instances of some samples, and too few of others.

When I run a quick calculation on the Google Sheet, I get the following counts:
![Label counts](https://joneslloyd.github.io/images/label-counts-sample.png){:width="500px"}

So maybe this unbalanced set of labels accounts for the lack of accuracy?

## But then again...

Given the multi-label classification task was essentially a warmup / toy project (before instance segmentation), maybe it isn't worth spending too much time on this...
