# Ths is some notes I want to shae in this new field of text to 3d scences


1. This is my notes I collected on Text to 3D scence. I don't claim any of this work as my own. I am just collecting it here for my own use. If you find this useful, please cite the original authors.

# ToDo
- [ ] Add IEEE References in my notes

# Blogs 

1. [Geometry in Text-to-Image Diffusion Models](https://akosiorek.github.io/geometry_in_image_diffusion/)

## 1 Geometry in Text To Image Diffusion Model Blogs 

[[Geometry in Text-to-Image Diffusion Models]]


# Diffusion Models
- [GAUDI](https://arxiv.org/abs/2207.13751) employs the diffusion modeling techniques used for image generation and applies them to 3D, which does result in better results than VAEs can provide. However, the model quality is still limited by the lack of high-quality 3D data.

# Why text to 3d scences does not work out 
- To do 3D modeling with NeRF (used in my work and in GAUDI above), you need several images and associated camera viewpoints for every scene, and, if you want to reach the scale of text-to-image models, you need millions if not billions of scenes in your dataset. This data does not exist on the Internet, because that’s not how people take (or post) pictures

## What are some promising direction  to build a huge text to 3d scence
- Nevertheless, video modeling with NeRF-based generative models is the most promising direction for future large-scale 3D models.


# Text-to-Image Models Know About Geometry
![[dreambooth.png]]

- If text-to-image models really know about 3D geometry, maybe we don’t need all that 3D data
	- Maybe we can just use the image models and either extract their 3D knowledge
		- correspond to extracting geometry from an image model ([DreamFusion](https://dreamfusion3d.github.io) and [Score Jacobian Chaining (SJC)](https://pals.ttic.edu/p/score-jacobian-chaining))
	- perhaps somehow nudge them to preserve geometry across multiple generated images
		- injecting geometry into an image model ([SceneScape](https://scenescape.github.io)), respectively
# Extracting Geometry from an Image Model

- That is, can we lift a generated 2D picture to a full 3D scene? The answer is, of course, yes. But why does it work? Because any 2D rendering of a 3D representation is an image, and if that representation contains a scene familiar to the image model (i.e. in the model distribution), that rendered image should have a high likelihood under the image model.
- But if we then manage to compute the gradients of the image model likelihood with respect to the 3D representation, we’ll be able to nudge the 3D representation into something that has a bit higher likelihood under that image model.
- Although they differ in derivations, both DreamFusion and SJC come up with novel image-space losses that capture the score (the derivative of the log probability) of a NeRF-rendered image under a pre-trained large-scale text-to-image diffusion model that is then back-propagated onto the NeRF parameters.
## Why Does Extracting Geometry Lead to Cartoonish Objects?
![[sjc_examples.png]]
1. First, the 3D representation is initialized with a random NeRF, which leads to rendered images that look like random noise. In this case, the diffusion model will denoise each of these images towards a different image as opposed to different views of the same scene. This makes it difficult to get the optimization off the ground, which may lead to training instabilities and lower final quality.
2. Second, this approach relies on classifier-free guidance with a very high guidance weight, which decreases the variance of the distribution (and its multimodality, see the end of this blog for a further discussion).

# Why Only Objects? What Happened to Full 3D Scenes?

- Optimization in such a case will lead to removing any objects that occlude the scene from the camera: in this case, it will remove everything, resulting in an empty scene. This is precisely why [GAUDI](https://arxiv.org/abs/2207.13751) models the joint distribution of indoor scenes and camera distributions (private correspondence with the authors).

# Refenerences

16. [[DreamFusion Text to 3d using 2D Diffusion]]
17. [[Score Jacobin Chaining (SJC)]]
18. [[RealFusion: 360° Reconstruction of Any Object from a Single Image]]
19. [[Magic3D High-Resolution Text-to-3D Content Creation]]
22. [[Musings on typicality]]
23. Go through the extracting geometry from an IMage Geometryu in Text model 
20. [[ScenceScape]]
21. [Geometry in Text-to-Image Diffusion Models](https://akosiorek.github.io/geometry_in_image_diffusion/)