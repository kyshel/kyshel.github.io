---
tags: [  Deep-learning]
title:  The dimension in deep learning
---

Never in my life I figure out the all kinds of dimension until scalar, vector, matrix, tensor was put together.

In deep learning, the dimension puzzled me for a long time. Some time I think one image is multi dimensions like 10000 dimensions or more. And some time I think is 2 dimension.

The key is SOMETIME. It means different situation. And in deep learning, out perspective is from dataset. 

From this view, scalar is 0D, vector is 1D, matrix is 2D, tensor is 3D or more. 

Like a dataset that contains 10 images, and each image's resolutiuon is  16 * 16 pixel. And the channel is gray. It means that every pixel's value is between 0 - 254.

In this dataset, the first dimesion is images serial number, we call sample here. the second is image_height, the third is  image_width. And the val of data point is the gray level of current pixel.


In keras, the dataset's dimension can be get by .ndim method. And the shape can be get by .shape method.

We can also change shape by .reshape. In the previous dataset, the shape is (samples, width, height), here is (10,16,16). If we want to change it to matrix, or to a table. We can change it to (10,16*16), and 10 means the number of samples, and 16 * 16 means features that each sample have.
