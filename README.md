# mines-anima

ComfyUI image workflows for **Anima** models. Each workflow is one settings node with the prompt and the settings you change, a saved result and a guide. Everything else is inside a subgraph.

## Workflows

| Workflow | What you get |
|---|---|
| **Mine's Anima** | Text to image, with optional hires fix, face detailer and hand detailer |
| **Mine's Anima LLLite** | An image that follows a control image (picture, sketch or lineart), with the same hires fix and detailers |
| **Mine's Anima Inpaint** | Redraws a masked area of a picture. The rest keeps its exact pixels. |

## Requirements

- **ComfyUI** with the Anima LLLite nodes built in. Tested on 0.45.4.
- **[mines-pack](https://github.com/kuromatcha/mines-pack)**, this project's node pack. Every workflow needs it.
- Three more node packs for the hires fix, the detailers and lineart extraction: see the [setup guide](docs/setup.md).

Model files are not included.

## Quick start

1. Follow the [setup guide](docs/setup.md): node packs and models.
2. Drag a file from [`workflows/`](workflows) into ComfyUI.
3. Pick your **model** and a **lora** on the settings node, write the prompt and run. For the first runs, turn hires fix and the detailers off.

## Docs

| | |
|---|---|
| [Setup](docs/setup.md) | Node packs, model downloads, first run |
| [Workflows](docs/workflows.md) | Every setting of each workflow, the values inside the subgraph, the test-then-render routine |

Each workflow also has a **Guide** note on the canvas with the short version.
