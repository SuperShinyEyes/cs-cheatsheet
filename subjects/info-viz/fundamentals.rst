============
Fundamentals
============

Terms
=====

Lie factor
##########
The “Lie Factor” is a value to describe the relation between the size of effect shown in a graphic and the size of effect shown in the data. To ensure the Integrity of a graphic, its Lie Factor should have a value between 0.95 and 1.05.

Space-time narrative
####################
It is a information visualization method where the time series data is visualized on spatial domain such as a map. One common example is Carte Figurative by Minard.

Design variation & data variation
#################################
Data variation is when your data varies which are the most cases. The problem occurs when one mixes design variation with data variation as it generates ambiguity and deception. A typical design variation is having different y-axes without clear notes.

Data ink maximization
#####################
Data-ink ratio is how much ink is spent on actual data. To maximize it, simply erase redundant & non data-ink. One example would be deleting grids on graphs or ticks.

Chartjunk
#########
A visualization is chartjunk if it contains redundant visualization elements which troubles viewers understanding the visualization. Types of chartjunk is ducks, vibrations and grids.

Graphical excellence
####################
* More ideas in shorter time.
* Don't waste space, ink
* Eliminate non-essentials

Tufte's principles
##################
- Show the data
- Induce the viewer to think about the substance
- Avoid distortion
- Data ink ratio maximization
- Make large data sets coherent
- Encourage the eye to compare different piece of data
- Reveal the data at several levels of detail
- serve a reasonably clear purpose: description, exploration, 
- Be integrated with the statistcal and verbal descriptions of a data set

Human perception
================

Gibson's affordance theory
##########################
We perceive possibilities for action in the environment, known as affordance. For instance, an open terrain affords walking; a stone on the ground affords tripping while walking.

Visual acuities
###############
Visual acuities are measurements of our ability to see detail. Simple acuities are restricted by the spacing of the receptor cells at the centre of the fovea. Superacuity is the ability to achieve better resolution by integrating information over space (or time).

Receptor cells
##############
rods: detect black/white/grey colours but not much detail
• function best in dim light
• located around the edges of the retina
• 120 million in each eye

cones: detect fine detail and colours 
• function best in bright light
• densely packed in fovea (centreof retina)
• 5 million in each eye

Acuity is at maximum at the centre of the fovea.

Attentiveness
#############
* Non-preattentive feature: Cound numbers of 3s.
* Preattentive feature: Size(Form)

Gestalt laws
############
* Proximity: Things that are near to each other appear to be grouped together. Place the data elements into proximity to emphasize connections between them.
* Similarity: Similar objects appear to be grouped together
* Good continuation: Visual complete objects are more likely to be constructed from visual elements that are smooth and continuous, rather than ones that contain abrupt changes in direction. It is one of the most powerful grouping principles. It's easier to perceive connections when contours run smoothly.

Sensory vs. arbitrary symbols
#############################
• sensory symbols
	• understandable without learning
	• processing is hard-wired and fast
	* resistant to instructional bias
	* cross-cultural

• arbitrary symbols
	• hard to learn and easy to forget (except when overlearned)
	• formally powerful
	• capable of rapid change
	* culture-specific

How light is perceived by eyes
##############################
Light falling on retina activates (1) receptor cells (i.e., rods and cones) which in turn activate (2) bipolar cells and then (3) ganglion cells through cascading photochemical reactions that transform the light into neural impulses, which carry visual information via the optic nerve to the visual processing areas in the visual cortex at the back of the brain where meaningful images are composed

Integral and separable dimensions
#################################
Separable features are perceived independent of each other such as size and color.

Integral features are perceived holistically such as width and height.

Use separable dimensions to encode different variables in glyphs.


Dimensionality Reduction
========================

What is PCA?
############
Mathematically PCA is about selecting most significant eigenvectors. The eigenvector with the largest eigenvalue is the principal component. The number of dimensions equal to the number of eigenvectors and the number of significant eigenvectors(or dimensions) one selects is determined by the user. It is useful as oftentimes the data may be more comprehendable when the PCA is conducted.

The steps are as following,

1. Standardize the data
2. Obtain the eigen vectors and eigenvalues.
3. Sort the eigen values in descending order and choose k largest eigen values. k is the number of dimensions
4. Construct the projection matrix W from the selected k eigenvectors
5. Transform the original dataset X via W to obtain a k-dimensional feature subspace Y.

When not to use PCA?
####################
When the dataset is non-linear.

`What is MDS? <https://youtu.be/GEn-_dAyYME>`_
##############################################
Metric Multi-Dimensional Scaling is similar to PCA. PCA creates plots based on **correlations** among samples while MDS creates plots based on **distances** among samples. That is the only difference.



Graphical visualization and navigation
======================================

What is a graph?
################
A graph is a visual representation of data which contrasts to tables. A graph is visual/pictorial so it is intuitive and easy to comprehend compared to tables. On the other hand, as graphs are visual it inherently cannot convey sharp accuracies as tables or raw data and it is easy to misguide viewers. Some of the popular graphs are bar, violin, heatmap and lines.


High-dimensional data
#####################
* small multiples with simple plots
* heatmaps
* parallel coordinates
* glyphs
* dimension reduction

Design principles for eyes
##########################
* physical luminance and perceived brightness can be quite different
• gray scale is bad at encoding absolute values, good at encoding relative values and shapes
• if outline of the shapes of objects is important:
	• background should have maximal contrast with foreground objects
• if it is important to see variations in grayscale:
	• background should have minimal contrast with foreground objects