# PomSeg: the persistent homology-based membrane segmentation
## Description
This is a 2D & 3D membrane segmentation tool based on persistent homology.

PomSeg employs persistent homology in a hierarchical manner. When provided with 2D image data, PomSeg utilizes grayscale intensity-based persistent homology to produce cell segmentation results. If a set of 2D slice images obtained from 3D cell imaging is the input, PomSeg can generate, from a set of 2D segmentation results, a 3D segmentation result using a 3D distance-based persistent homology calculation.

![alt text](https://github.com/TopologicalBird/PomSeg/blob/main/images/pomseg_img.png?raw=true)

The embryo image shown in the jupyter notebook was provided by Dr. Dimitri Fabrèges

used in the following article:

https://doi.org/10.1101/2023.01.24.525420

Joint work with Takafumi Ichikawa (Kyoto Univ.) & Yusuke Imoto (Kyoto Univ.)

## Method
### Preprocessing
First, we preprocess the image slice by slice using Ridge filter. This allows us to enhance the membrane parts.
### 2D persistent homology
We apply sublevel filtration persistent homology to the 2D slices.

From the resulting 0th persistence diagram, we select generators with persistence values greater than a user-defined persistence parameter.

This persistence reflects the intensity differences between the membrane and the interior of the cell. The persistence parameter sets the minimum intensity difference required for a structure to be detected. 

We construct binary mask images for the cell region using the inverse analysis.
### 3D binary image construction
By piling up the 2D binary masks, we make a 3D binary image with cell parts being white.
### 3D persistent homology
We distance transform the 3D binary image and apply sublevel filtration persistent homology.

From the resulting 0th persistence diagram, we select generators with persistence and the absolute value of birth larger than specified parameters, determined by the user.

In this context, a birth value reflects the first moment when the center of a cell enters the filtration, thus corresponding to the cell size.

On the other hand, the persistence value indicates how long a structure remains before merging with another. Therefore, if a cell significantly overlaps with another, its persistence will be smaller, indicating the extent of overlap. 

We retrieve the birth positions of the selected generators.

### Finalizing segmentation
We use watershed method with the birth positions above as markers to finalize the segmentation.

## Related Paper
For more detailed explanations, see the paper below.

PAPER LINK HERE!

## Persistent homology calculation
We use HomCloud for the persistent homology calculation. You can install it from the link below.

https://homcloud.dev/index.en.html
