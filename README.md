# Road Extraction from Areal-Imagery
## Motivation
In our rapidly modernizing world, many areas of our Earth's land cover are being urbanized or turned into adjacent land use types like farms to support our growing population. These types of activities have a large negative impact on the precarious ecological and climate states of our planet.

Specifically, many ancient forests and other carbon sinks are being turned into cities and other mono-culture land use types.

Developing a method to monitor and track these activities can be crucial in gathering evidence and enacting policies to safeguard our planet's most crucial natural land cover types. Satellite imagery and other remote sensing data are invaluable in the fight against climate change in areas such as fighting deforestation, land use carbon accounting, icecap monitoring, and many others.
## Problem
For this project, I will develop a artificial neural network trained on satellite imagery to classify the different land use types within that satellite image. This is a classic multiclass semantic segmentation problem on image data. 

Additionally, I wanted to use this project to gain hands-on valuable experience in working with satellite imagery data, image processing technqiues, and working with CNNs.
## Dataset Discovery
The dataset used was obtained from [Kaggle](https://www.kaggle.com/balraj98/deepglobe-land-cover-classification-dataset) and the original source is from the [Land Cover Classification Track in DeepGlobe's 2018 Challenge](https://competitions.codalab.org/competitions/18468).

Data:
*   The training data for Land Cover Challenge contains 803 satellite imagery in RGB, size 2448x2448. 
  * 2448x2448 is too large for our system to handle so it will need to be cut down into smaller patches per image.
*   The imagery has 50cm pixel resolution, collected by DigitalGlobe's satellite.
*   The dataset contains 171 validation and 172 test images (but no masks). 
  * We will combine the original validation and test images into a test dataset, split the train dataset, and appropriate a portion for validation with masks.

Label:
*   Each satellite image is paired with a mask image for land cover annotation. The mask is a RGB image with 7 classes of labels, using color-coding (R, G, B) as follows.
    * Urban land: 0,255,255 - Man-made, built up areas with human artifacts (can ignore roads for now which is hard to label)
    * Agriculture land: 255,255,0 - Farms, any planned (i.e. regular) plantation, cropland, orchards, vineyards, nurseries, and ornamental horticultural areas; confined feeding operations.
    * Rangeland: 255,0,255 - Any non-forest, non-farm, green land, grass
    * Forest land: 0,255,0 - Any land with x% tree crown density plus clearcuts.
    * Water: 0,0,255 - Rivers, oceans, lakes, wetland, ponds.
    * Barren land: 255,255,255 - Mountain, land, rock, dessert, beach, no vegetation
    * Unknown: 0,0,0 - Clouds and others
* File names for satellite images and the corresponding mask image are id _sat.jpg and id _mask.png. id is a randomized integer.
## Methodology Overview
The overall methodology of this expriment is as follows:

1.   Download and explore data.
2.   Load data.
3.   Preprocess images where necessary.
4.   Select and build appropriate network architecture.
5.   Train on data and mask labels.
6.   Test performance on validation dataset.
7.   Tune parameters and hyperparameters of model where necessary.
8.   Repeat steps 6 & 7 until we have reached satisfactory results.
9.   Select the final trained model and test on test data.

> Data citation:
```
@InProceedings{DeepGlobe18,
 author = {Demir, Ilke and Koperski, Krzysztof and Lindenbaum, David and Pang, Guan and Huang, Jing and Basu, Saikat and Hughes, Forest and Tuia, Devis and Raskar, Ramesh},
 title = {DeepGlobe 2018: A Challenge to Parse the Earth Through Satellite Images},
 booktitle = {The IEEE Conference on Computer Vision and Pattern Recognition (CVPR) Workshops},
 month = {June},
 year = {2018}
}```
