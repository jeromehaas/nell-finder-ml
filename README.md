# [ML] Nell Finder

![Python](https://img.shields.io/badge/Python-3-blue)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626)
![Ultralytics](https://img.shields.io/badge/Ultralytics-YOLO-black)
![YOLO](https://img.shields.io/badge/YOLO-Segmentation-111111)
![ONNX](https://img.shields.io/badge/ONNX-Export-005CED)

Nell Finder TR is the training repository for Swiss playing card recognition. It uses a Jupyter notebook and Ultralytics YOLO to prepare data, train a segmentation model, test predictions on images, and optionally export the trained model for use in the separate `nell-finder-ui` project.

## Requirements

- Python 3
- pip
- JupyterLab
- A dataset of images and matching YOLO polygon label files

## Installation

1. Install the dependencies:

```bash
pip install -r requirements.txt
```

2. Start JupyterLab:

```bash
jupyter lab
```

3. Open `src/nell_finder_local.ipynb` and run the notebook from top to bottom.

## Data

Put your files into this structure:

```text
nell-finder-tr/
├── data/
│   ├── input/
│   │   ├── images/
│   │   └── labels/
│   └── tmp/
├── dist/
├── runs/
└── src/
    ├── classes.txt
    └── nell_finder_local.ipynb
```

Notes:

- Each label file must match the image filename stem.
- `classes.txt` contains one class name per line.
- The notebook regenerates `data/tmp/data.yaml`, `data/tmp/train.txt`, and `data/tmp/val.txt` automatically.

## Notebook Flow

The notebook walks through these steps:

1. Check the environment and choose the best available device.
2. Find the project paths automatically.
3. Inspect the input data.
4. Create a reproducible train/validation split.
5. Train a YOLO segmentation model.
6. Test the trained model on one image.
7. Optionally export the model to ONNX.

## Outputs

Important output locations:

- `data/tmp/` for generated training helper files
- `runs/` for Ultralytics training and prediction runs
- `dist/` for optional exported models

The trained weights used by the notebook are loaded directly from the run folder, for example `runs/<run-name>/weights/best.pt`.

## UI Integration

If you export an ONNX model, you can use it in the separate `nell-finder-ui` repository.

That frontend project expects a model file that is compatible with its browser inference pipeline. In practice, this means the exported model should replace the ONNX file used by the UI project.

## Notes

- The notebook uses timestamped run names, so training runs do not overwrite each other.
- Images without matching labels are skipped automatically during dataset preparation.
- The export step is optional. Training and notebook-based testing work without it.
