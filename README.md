# mines-anima

ComfyUI image workflows for **Anima** models. Each one is a single settings node with the prompt and the settings you change, a saved result and a guide; everything else is inside the subgraph.

| Workflow | Makes |
|---|---|
| **Mine's Anima** | Text to image, with optional hires fix, face detailer and hand detailer |
| **Mine's Anima LLLite** | Image that follows a control image (picture, sketch or lineart) through the Anima LLLite, with the same hires fix and detailers |
| **Mine's Anima Inpaint** | Redraws a masked area of a picture through the Anima LLLite inpainting model; the rest keeps its exact pixels |

Each workflow has a **Guide** note with every setting and a test-then-render routine.

## Quick start

1. Install the node packs and models below.
2. Drag a file from [`workflows/`](workflows) into ComfyUI.
3. Write the prompt and run. For the first runs, turn hires fix and the detailers off.

## Node packs

| Pack | Used by | Install |
|---|---|---|
| **mines-pack** (*mine's pack*) | All: prompt, pick style, inpaint crop/stitch, settings-node layout | `git clone https://github.com/kuromatcha/mines-pack.git` into `ComfyUI/custom_nodes/` |
| **ComfyUI Impact Pack** and **Impact Subpack** | Anima, LLLite: face and hand detailers | ComfyUI-Manager |
| **SesquiLSR** | Anima, LLLite: hires fix latent upscale | https://github.com/LoganBooker/SesquiLSR |
| **comfyui_controlnet_aux** | LLLite: lineart extraction | ComfyUI-Manager |

The LLLite nodes (Model Patch Loader, Anima LLLite Apply) are part of ComfyUI itself.

## Models

| File | Folder | Used by |
|---|---|---|
| An Anima checkpoint, e.g. `anima-base-v1.0.safetensors` | `models/diffusion_models` | All |
| `qwen_3_06b_base.safetensors` | `models/text_encoders` | All |
| `qwen_image_vae.safetensors` | `models/vae` | All |
| `anima-lllite-any-test-like-v2.safetensors` | `models/model_patches` | LLLite |
| `anima-lllite-inpainting-v2.safetensors` | `models/model_patches` | Inpaint |
| `segm/Anzhc Face seg 640 v4 y11n.pt` | `models/ultralytics/segm` | Anima, LLLite (face) |
| `bbox/hand_yolov8s.pt` | `models/ultralytics/bbox` | Anima, LLLite (hands) |
| `sam_vit_b_01ec64.pth` | `models/sams` | Anima, LLLite (hands) |

A LoRA is optional: pick one on the settings node, or set **lora strength** to 0.

Model files are not included.
