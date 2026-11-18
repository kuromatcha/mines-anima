# Workflows

Setting names match the settings node. Values in **bold** are what the workflows are saved with. Each workflow's **Guide** note has the short version of this page.

- [Mine's Anima](#mines-anima)
- [Mine's Anima LLLite](#mines-anima-lllite)
- [Mine's Anima Inpaint](#mines-anima-inpaint)
- [Routine](#routine)

## Shared by all three

| Setting | Notes |
|---|---|
| model | The Anima checkpoint. |
| lora, lora strength | Applies to every stage. Strength 0 = off, but the file must exist: see [setup](setup.md#lora). |
| steps, cfg | **30**, **4**. |

Inside the subgraph (double-click the settings node to open it):

- Sampler **er_sde**, scheduler **simple**, shift **3**.
- Seeds are random. To keep one, set *Sample* to *fixed* (also *Sample hires* and the detailers).
- The text encoder, VAE, LLLite file and detectors are picked on the *Load …* nodes.
- The *Previews* group keeps each stage's picture.

## Mine's Anima

Text to image.

### Prompt

| Field | Notes |
|---|---|
| quality | Shared by both styles. |
| style 1, style 2 | Artist tags. Each style is encoded as quality + that style + positive prompt. An empty style is just quality + positive prompt. |
| positive prompt, negative prompt | |

**style 1** is used by the first pass. **hires style**, **face style** and **hand style** pick 1 or 2 for the later stages. A style that no stage picks is not encoded.

### Settings

| Setting | Saved value | Notes |
|---|---|---|
| width, height | **1024 × 1024** | Size of the first pass. |
| batch size | **1** | Pictures per run. |
| hires fix | **on** | Upscales the latent by **hires scale** and resamples at **hires denoise** (15 steps). Off is much faster, for testing. |
| hires scale | **1.5** | |
| hires denoise | **0.4** | |
| face detailer | **on** | Redraws faces at 1024. Turn off if the picture has none. |
| face denoise | **0.4** | Higher = more change. |
| hand detailer | **off** | Redraws hands at 1024. |
| hand denoise | **0.2** | Higher = more change. |

Other detailer values (guide size, detection, steps) are on the *Face detailer* and *Hand detailer* nodes inside the subgraph.

## Mine's Anima LLLite

An image that follows a control image. The control image sets the composition; the prompt sets everything else. The prompt fields and the hires fix and detailer switches are the same as in Mine's Anima.

### Control image

- A picture, a sketch or lineart.
- **extract lineart**: on for a finished picture, which is turned into lineart first. Off if the image already is lineart, a sketch or grayscale.
- The lineart preview inside the subgraph shows what the LLLite gets.
- Describe in the prompt what the lines do not say: colors, materials, lighting, background.

### Settings

| Setting | Saved value | Notes |
|---|---|---|
| megapixels | **1** | Size of the first pass. The output takes the control image's shape. |
| extract lineart | **on** | See above. |
| control strength | **0.8** | Lower = looser lines. |
| control end | **0.6** | Share of the steps the lines are held for. Lower = more freedom in details, 1 = traced. |
| hires fix, hires scale, hires denoise | **on**, **1.5**, **0.4** | As in Mine's Anima. |
| face detailer, hand detailer | **on**, **on** | Denoise is set inside the subgraph: 0.3 for faces, 0.2 for hands. |

## Mine's Anima Inpaint

Redraws a masked area. The area is cut out with its surroundings, redrawn at full size and blended back. Only the painted area changes; the rest keeps its exact pixels.

### Mask

- Right-click the image → **Open in MaskEditor**, paint the area to redraw, save.
- Cover the whole thing you replace and a little around it.
- Running without a painted mask stops with *The mask is empty*.

### Prompt

- **quality**, **style** (artist tags) and **positive prompt** are joined in that order; empty ones are skipped.
- Describe the cut-out as it should look afterwards, not the edit: `red dress`, not `change the dress to red`.
- Keep the artist tags and LoRA of the original picture, so the new part matches.
- To remove something, describe what is behind it (`wall, window`) and put the thing in the negative prompt.

### Settings

| Setting | Saved value | Notes |
|---|---|---|
| model | | The checkpoint the picture was made with. The LLLite was trained on anima-base-v1.0; finetunes work. |
| megapixels | **1** | Size the cut is redrawn at. Up to 2.3 for a large area. |
| context | **2** | Size of the cut as a multiple of the masked area. Higher matches the picture better; lower gives more detail. Small masks get at least 256 px. |
| grow mask | **8** | Widens the mask, in pixels. |
| blend edge | **16** | Soft edge when the cut is put back, in pixels. Raise it if a seam shows. |
| control strength | **1** | How strongly the LLLite ties the new part to its surroundings. 0.7–0.9 is freer. |
| denoise | **1** | 1 replaces the area. 0.5–0.7 keeps the shapes under the mask and fixes details (hands, faces). |

## Routine

**Anima and LLLite**

1. Test with hires fix and the detailers off until the composition and colors are right.
2. Fix the seed in *Sample*, turn hires fix and the detailers on, and run.

**Inpaint**

1. Paint the mask, write the prompt, run.
2. Run again for another seed. Wrong shape: change the prompt. Visible seam: more blend edge or context.
3. For the next area, load the result as the image and paint again.
