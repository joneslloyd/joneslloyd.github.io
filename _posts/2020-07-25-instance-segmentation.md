# Instance segmentation

We've now started (via Labelbox) to segment instances of the salmon lice. Our approach is broadly:

- Segment each louse as an individual instance, and assign the label "louse"
- Add sub-classifications of either "Male" or "Female" and "Adult" or "Preadult" to each
- Add an optional further sub-classification of the sub-species

Once we have sufficient segmented and labelled image data, we can then start training.

I've looked into different architectures, and while FastAI may do the trick, [Mantis Shrimp](https://spectrum.chat/mantis){:target="_blank" rel="noopener"} (an agnostic object detection framework) looks most promising because it lends itself more directly to the task in hand.

My next post should hopefully be a 'dive in' of getting started with instance segmentation. Until then, more images need to be segmented, manually (by me) and have their sub-classifications applied (by Tina).

That's all for now!