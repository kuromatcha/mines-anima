# mines-image

ComfyUI image workflows for **Anima** models, plus background removal for anime and illustration. Each workflow is one settings node with the prompt and the settings you change, a saved result and a guide. Everything else is inside a subgraph.

## Workflows

| Workflow | What you get |
|---|---|
| **Mine's Anima** | Text to image, with optional hires fix, face detailer and hand detailer |
| **Mine's Anima LLLite** | An image that follows a control image (picture, sketch or lineart), with the same hires fix and detailers |
| **Mine's Anima Inpaint** | Redraws a masked area of a picture. The rest keeps its exact pixels. Paint the mask or load a saved one. |
| **Mine's Cutout** | A transparent PNG of the character, upscaled 4x |

## Requirements

- **ComfyUI** with the Anima LLLite nodes built in. Tested on 0.45.4.
- **[mines-pack](https://github.com/kuromatcha/mines-pack)**, this project's node pack. Every Anima workflow needs it.
- Three more node packs for the hires fix, the detailers and lineart extraction, and [ComfyUI-RMBG](https://github.com/1038lab/ComfyUI-RMBG) for background removal: see the [setup guide](docs/setup.md).

Model files are not included.

## Quick start

1. Follow the [setup guide](docs/setup.md): node packs and models.
2. Drag a file from [`workflows/`](workflows) into ComfyUI.
3. Pick a **lora** on the settings node (any file you have; strength 0 keeps it off), write the prompt and run. For the first runs, turn hires fix and the detailers off. **Cutout** needs no prompt: load a picture and run.

## Docs

| | |
|---|---|
| [Setup](docs/setup.md) | Node packs, model downloads, first run |
| [Workflows](docs/workflows.md) | Every setting of each workflow, the values inside the subgraph, the test-then-render routine |

Each workflow also has a **Guide** note on the canvas with the short version.
