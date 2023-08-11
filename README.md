# Textual Inversion

[Textual Inversion](https://arxiv.org/abs/2208.01618) is a technique for capturing novel concepts from a small number of example images. 
While the technique was originally demonstrated with a [latent diffusion model](https://github.com/CompVis/latent-diffusion), it has since been applied to other model variants like Stable Diffusion. 
The learned concepts can be used to better control the images generated from text-to-image pipelines. 
It learns new “words” in the text encoder’s embedding space, which are used within text prompts for personalized image generation.

<table style="width:100%">
  <tr>
    <th>Architecture overview from the Textual Inversion</th>
  </tr>
  <tr>
    <td><img src="training.jpeg" alt="Architecture" height="400"></td>
</table>

Usually, text prompts are tokenized into an embedding before being passed to a model, which is often a transformer. 
Textual Inversion does something similar, but it learns a new token embedding, v*, from a special token S* in the diagram above. 
The model output is used to condition the diffusion model, which helps the diffusion model understand the prompt and new concepts from just a few example images.

To do this, Textual Inversion uses a generator model and noisy versions of the training images. 
The generator tries to predict less noisy versions of the images, and the token embedding v* is optimized based on how well the generator does. 
If the token embedding successfully captures the new concept, it gives more useful information to the diffusion model and helps create clearer images with less noise. 
This optimization process typically occurs after several thousand steps of exposure to a variety of prompt and image variants.

# Self-training