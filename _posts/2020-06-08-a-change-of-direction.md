# Multi-label classification: An update

Long story short, this was never going to work with the fuzziness of the dataset's labelling.

The issue was that, in a load of petri-dishes containing soem biofouling, some anemones, some fish feed (etc), the threshold for "This contains biofouling" was always going to be arbitrary. Where there's a tiny speck of biofouling and we're saying "This contains no biofouling", we're giving the neural network a really hard time in figuring out what exactly it's supposed to be abtracting from the images to form its notions of "biofouly-ness", "licey-ness", "feedy-ness", etc.

Or to put it another way:

We are forcing the neural network to pull in two directions. Given that nearly all of the images contained a little of each thing, in entirely different forms, locations, and amounts, but our class labelling was absolute ("This DOES contain X", and "This DOESN'T contain X"), the network is being given mixed messages about what it is that it's really supposed to be learning. Consequently it can't form solid "rules" for the multi-label classification, and ultimately, fails at the task.

# But... what next?

## Instance segmentation

A similar but related problem is that of lice classification.

Given an image containing lice at various stages, like this:
![Lice samp,e](https://joneslloyd.github.io/images/lice-sample.png){:width="500px"}

...can we figure out, for each louse:

- Louse gender
- Louse stage (based on size)
- Mean grey value of each louse (convert to 8bit greyscale – to measure ‘how light/dark’ the louse is)
- Louse area (size)

## Approach

After speaking with some incredibly smart and helpful people (namely @digitalspecialists and @lgvaz) the general approach I'm going to take is:

- Use CV2 to identify regions (lice) in an image. CV2 can also be used for mean grey value
- Use a U-Net to correctly label the lice in a novel image (Male/Female and Preadult/Adult)

Stay tuned!