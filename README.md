# &#128047; TIGER: A Unified Generative Model Framework for Multimodal Dialogue Response Generation


# Demo

![demo](figs/screenshot/demo_system.png)

We implemented a multimodal dialogue system based on TIGER, as depicted in figure above.    

&#10071;**Note:** It's worth mentioning that our research focuses on open-domain multimodal dialogue response generation. However, the system may not possess perfect instruction-following capabilities. Users can treat it as a companion or listener, but using it as a QA system or AI painting generator is not recommended.

Our system offers various modifiable components.  
- For the textual dialogue response generator, users can can choose decoding strategies and adjust related parameters.  
- For the Text-to-Image translator, users can freely modify prompt templates and negative prompt to suit different requirements. Default prompt templates and negative prompts are provided, enhancing the realism of generated images (for more details, refer to Appendix \ref{appendix:prompt}).

## Examples

|   |   |
:-------------------------:|:-------------------------:
![conv1](figs/screenshot/conversation1.png) | ![conv2](figs/screenshot/conversation2.png)
![conv3](figs/screenshot/conversation3.png) | ![conv4](figs/screenshot/conversation4.png)

# Supplementary Instructions

## Image Descriptions

Image descriptions are used for the training of two components (textual dialogue response generator and Text-to-Image translator). For the textual dialogue response generator, accurate and detailed image descriptions facilitate the generation of more contextualized image descriptions in inference. For the Text-to-Image translator, high-quality image-text pairs are necessary to improve the performance of Stable Diffusion in fine-tuning stage. According to our preliminary experience, the performance will be degraded if model is trained with the image descriptions provided by PhotoChat. In addition, it is also necessary that we use Gigapixel to further improve the quality of the images.

<table align="center">
  <tr>
    <td align="center"><b></b></td>
    <td align="center"><b>Images</b></td>
    <td align="center"><b>Descriptions</b></td>
  </tr>
  <tr>
    <td rowspan="2" align="center">(a)</td>
    <td rowspan="2" align="center"><img src="figs/supplement/description_1.jpg" height="120px" title="(a)"></td>
    <td>PhotoChat: The photo has your aunt Kailey. Objects in the photo: Woman, Face</td>
  </tr>
  <tr>
    <td>BLIP-2: a man giving a presentation in front of a group of people</td>
  </tr>
  <tr>
    <td rowspan="2" align="center">(b)</td>
    <td rowspan="2" align="center"><img src="figs/supplement/description_2.jpg" height="120px" title="(b)"></td>
    <td>PhotoChat: Objects in the photo: Animal, Carnivore, Cat</td>
  </tr>
  <tr>
    <td>BLIP-2: a cat sitting on the floor with its paw on its head</td>
  </tr>
  <tr>
    <td rowspan="2" align="center">(c)</td>
    <td rowspan="2" align="center"><img src="figs/supplement/description_3.jpg" height="120px" title="(c)"></td>
    <td>PhotoChat: Objects in the photo: Building, Chair, Drink</td>
  </tr>
  <tr>
    <td>BLIP-2: a bar with neon lights and a sign that says be amazing</td>
  </tr>
</table>

Comparison of image descriptions provided by PhotoChat with those generated by BLIP-2. (a), (b), and (c) correspond to images in Figure \ref{fig:description}, respectively.


The table above shows several images from PhotoChat and demonstrates that the image descriptions generated by BLIP-2 are more accurate and detailed than those provided by PhotoChat. Specifically, image descriptions generated by BLIP-2 can express: 
1. character's actions (e.g., "giving a presentation" in (a), "with its paw on its head" in (b)). 
2. the relative positional relationships between objects (e.g., "in front of" in (a)). 
3. scene information (e.g., "with neon lights" in (c)). 

Therefore, we use these new image descriptions instead of those provided by PhotoChat.  

## Prompts

The image descriptions generated by the textual dialogue generator are brief sentences. Although they are context-sensitive, a short sentence will not inspire Stable Diffusion to generate a perfect image. Thus we add some extra prompts following the generated image descriptions, that is, applying some prompt templates. For various categories of descriptions (e.g., buildings, landscapes, portraits), the use of corresponding prompt templates would significantly improve the quality of the images. However, our research topic is multimodal dialogue response generation, not just Text-to-Image. Specifically, the description is generated and used inside the model, from which we cannot decide which category it belongs to. Here we simply divide all descriptions into "Human" and "Others". Prompt templates are shown in Table \ref{tab:extra_prompts}. 

The goal of using both prompt templates and negative prompt is to stabilize the generated image in the style we desire and to improve the quality of the image. 

# Getting Start

## Hardware

&#11088; A GPU with 24G of memory is enough for the demo.  

## Installation

### 1. Prepare the code and the environment
```
git clone https://github.com/friedrichor/TIGER.git
cd TIGER/
conda env create -f environment.yml
conda activate tiger
```

### 2. Prepare the model weights

&#10024; Please download our model weights from [here](https://drive.google.com/drive/folders/1ulc4X0yzJHQNFZJ2nyH5H9ZatZPkzZTC?usp=sharing) (Google Drive). The final weights would be in a single folder in a structure similar to the following:

```
TIGER
├── demo
│   └── ...
├── model_weights
│   ├── tiger_t5_base_encoder.pth
│   ├── tiger_dialogpt_medium.pth
│   └── tiger_stable_diffusion
│       └── ...
├── tiger
│   └── ...
├── utils
│   └── ...
├── demo.py
...
```

&#128216; For Text-to-Image Translator's weights, we have already uploaded it to Hugging Face, so you don't need to download it locally now. More details can be sourced from [friedrichor/stable-diffusion-2-1-realistic](https://huggingface.co/friedrichor/stable-diffusion-2-1-realistic).

## Launching Demo Locally

```
python demo.py --config demo/demo_config.yaml
```


