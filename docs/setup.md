# Setup

1. [Install ComfyUI](#1-install-comfyui)
2. [Install the node packs](#2-install-the-node-packs)
3. [Download the models](#3-download-the-models)
4. [First run](#4-first-run)

You only need the parts for the workflows you want. The **Needed by** columns say which.

## 1. Install ComfyUI

Use a current ComfyUI. The LLLite nodes (*Model Patch Loader*, *Anima LLLite Apply*) are part of ComfyUI itself, so an old version shows them as missing. So is the *If/Else Switch* that picks the inpaint mask.

Tested on ComfyUI 0.45.4 (Comfy Desktop, PyTorch 2.12.1 + CUDA 13.0) with an RTX 5060 Ti 16 GB.

## 2. Install the node packs

Install with ComfyUI-Manager (**Install Custom Nodes**), or `git clone` into `ComfyUI/custom_nodes/`. Restart ComfyUI afterwards.

| Pack | Needed by | Used for | Install |
|---|---|---|---|
| [mines-pack](https://github.com/kuromatcha/mines-pack) | All | Prompt and pick style nodes, inpaint crop and stitch, settings-node layout | `git clone https://github.com/kuromatcha/mines-pack.git` (not in Manager) |
| [ComfyUI Impact Pack](https://github.com/ltdrdata/ComfyUI-Impact-Pack) and [Impact Subpack](https://github.com/ltdrdata/ComfyUI-Impact-Subpack) | Anima, LLLite | Face and hand detailers | Manager |
| [SesquiLSR](https://github.com/LoganBooker/SesquiLSR) | Anima, LLLite | Hires fix (latent upscale) | git |
| [comfyui_controlnet_aux](https://github.com/Fannovel16/comfyui_controlnet_aux) | LLLite | Lineart extraction | Manager |

**Mine's Anima Inpaint** needs only mines-pack.

## 3. Download the models

Folders are inside `ComfyUI/models/`.

### All workflows

| Folder | File | Size | Source |
|---|---|---|---|
| `diffusion_models/` | An Anima checkpoint, e.g. `anima-base-v1.0.safetensors` | 4.2 GB | [circlestone-labs/Anima](https://huggingface.co/circlestone-labs/Anima) → `split_files/diffusion_models/` |
| `text_encoders/` | `qwen_3_06b_base.safetensors` | 1.2 GB | same repo → `split_files/text_encoders/` |
| `vae/` | `qwen_image_vae.safetensors` | 0.25 GB | same repo → `split_files/vae/` |

The workflows are saved with `anima-base-v1.0.safetensors`. Anima finetunes work too: pick yours on the settings node.

### LLLite and Inpaint

| Folder | File | Size | Needed by | Source |
|---|---|---|---|---|
| `model_patches/` | `anima-lllite-any-test-like-v2.safetensors` | 16 MB | LLLite | [Comfy-Org/Anima-LLLite](https://huggingface.co/Comfy-Org/Anima-LLLite) → `model_patches/` |
| `model_patches/` | `anima-lllite-inpainting-v2.safetensors` | 66 MB | Inpaint | same |

### Detailers (Anima and LLLite)

| Folder | File | Size | Used for | Source |
|---|---|---|---|---|
| `ultralytics/segm/` | `Anzhc Face seg 640 v4 y11n.pt` | 6 MB | Face detailer | [Anzhc/Anzhcs_YOLOs](https://huggingface.co/Anzhc/Anzhcs_YOLOs) |
| `ultralytics/bbox/` | `hand_yolov8s.pt` | 23 MB | Hand detailer | [Bingsu/adetailer](https://huggingface.co/Bingsu/adetailer) |
| `sams/` | `sam_vit_b_01ec64.pth` | 375 MB | Hand detailer | ComfyUI-Manager → **Model Manager**, search `sam_vit_b` |

Download all three, also if you keep a detailer switched off. ComfyUI checks every file a workflow names before it runs: while one is missing, the run finishes without saving a result.

### LoRA

A LoRA is optional, but the **lora** field must name a file you have. The workflows are saved with the placeholder `your_lora.safetensors` at strength 0, and ComfyUI refuses to run while the named file is missing, even at **lora strength** 0.

- With a LoRA: pick it on the settings node and set **lora strength**.
- Without one: pick any LoRA file you have and leave **lora strength** at 0. If you have no LoRA file at all, open the subgraph and bypass *Load LoRA* (select it, Ctrl+B).

## 4. First run

1. Start ComfyUI and drag a file from [`workflows/`](../workflows) onto the canvas. No red "missing node" boxes should show. If some do, a node pack from step 2 is missing.
2. On the settings node, pick a **lora** (see above), and your **model** if it is not the base checkpoint.
3. **Mine's Anima:** write the prompt, or run the example prompt. **LLLite:** also load a control image. **Inpaint:** load a picture, right-click it → **Open in MaskEditor**, paint the area to redraw and save, or turn on **use mask file** and pick a saved mask on *Mask file*. The image loaders are saved with ComfyUI's own `example.png`.
4. Turn **hires fix**, **face detailer** and **hand detailer** off for the first runs, then run.
5. The picture is saved by the **Result** node. Each stage's picture is kept in the *Previews* group inside the subgraph.

What every setting does: [workflows](workflows.md).
