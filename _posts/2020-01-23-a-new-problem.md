# A new problem!

In looking for a real-world challenge to solve as part of my machine learning-learning, I spoke to [a friend in Norway](https://loop.frontiersin.org/people/505062/overview) who does a lot of acadmic and post-doctoral work with salmon. I don't pretend to understand most of it, but one particular task that she undertakes sounded like a good candidate for a ML project:

Farmed salmon get lice. This is a bad thing. Another type of fish ('cleaner fish') eat these lice, so are introduced into the salmon's underwater cages to keep the lice at bay.

The effectiveness of these cleaner fish can be measured by looking at the number of lice in their stomachs.

Currently, this is done by taking a photo and manually counting the lice.

But.... What if a neural network could do this job?

That's what I'm trying to find out.

## Background and preparation

While I wait on the first photos, which will provide a lot of context, I've been thinking about the ways in which this problem could be approached.

I read a [great series of blog posts by Josh Varty](https://github.com/JoshVarty/ImageClassification/blob/master/3_CountingAgain.ipynb), in which he detailed his Fast AI project that counted dots. He approached it as an [image classification](https://towardsdatascience.com/fastai-image-classification-32d626da20) problem.

But as Josh says in his posts, this isn't *really* counting, because the neural network is realy just learning every possible 'shape' that "X dots" could be in an image. Plus, the range of numbers needs to be pre-defined (it being a classification problem and all).

Not really ideal.

So, approaching it as a regression problem makes more sense. As [@btahir](https://medium.com/r/?url=http%3A%2F%2Ftwitter.com%2Fbtahir) says: "*This makes sense if you think about using classification, you would be penalizing an output of 32 vs the ground truth of age 31 the same as if the model had predicted age 99. We want to let the model know that the closer it got to the true age, the better*".

And Jeremy Howard also said, in response to a question on the FastAI forums: "*[I don't think our normal convnets will work well for that. You'll need to use object detection](https://medium.com/r/?url=https%3A%2F%2Fforums.fast.ai%2Ft%2Fshare-your-work-here%2F27676%2F540%3Fu%3Djoneslloyd)*".

I did later realise that Josh produced [another post](https://github.com/JoshVarty/ImageClassification/blob/master/4_CountingRegression.ipynb) where he did approach it as a regression problem, which is exactly what I'd like to do.

However, my problem will be a little more complex – The images will have a lot of noise, and some of the lice will be overlapping (etc).

So right now (and after [consulting much wiser people](https://forums.fast.ai/t/novice-how-to-approach-a-problem-image-classification-image-segmentation/61566/3) on the Fast AI forums), I'm thinking that this needs to be approached as a two-stage problem:

- Identifying the lice
- Counting them

Perhaps two neural networks? Or a CNN with some sort of regression output?

I'll keep digging and see what I find...
