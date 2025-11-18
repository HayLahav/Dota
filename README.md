# Dota
DOTA-VLM: Automated Annotation of Aerial Imagery Using Vision–Language Models
A Vision–Language pipeline for enriching, validating, and auto-annotating the DOTA aerial dataset with captions, attributes, and contextual metadata.
📌 Table of Contents

Overview

Key Features

Project Architecture

Dataset

Installation

Usage

1. Run Oriented Object Detector

2. Crop Objects

3. Generate VLM Annotations

4. Export DOTA-VLM JSON

Annotation Types

Prompt Templates

Examples

Evaluation

Project Roadmap

Citation

License

🔍 Overview

DOTA-VLM is a semi-automatic annotation framework that enriches the original DOTA dataset using a Vision–Language Model (VLM) such as:

LLaVA / LLaVA-NeXT

BLIP-2 / BLIP-3

Florence-2

OWL-ViT

This project adds fine-grained semantic metadata to DOTA, including:

Object attributes

Spatial relations

Image captions

Activity descriptions

Confidence & ambiguity indicators

The output is a COCO-style JSON with both the original annotations and the new VLM-generated metadata.

🌟 Key Features

Automatic object attribute generation (size, orientation, activity, appearance).

Scene captioning for each aerial image.

Spatial relationship extraction between objects.

Zero-shot class verification using a VLM.

Quality & uncertainty estimation for weak labels.

Plug-and-play detector support: YOLO-OBB, Oriented RCNN, DiffusionDet, DINO-OBB.

Configurable prompting system for multi-modal annotation.

🏗️ Project Architecture
          ┌──────────────────────┐
          │   DOTA Image (RGB)   │
          └───────────┬──────────┘
                      │
                      ▼
       ┌────────────────────────────┐
       │ Oriented Object Detector   │
       │ (YOLO-OBB / DiffusionDet) │
       └───────────┬───────────────┘
                   │  Rotated Boxes
                   ▼
       ┌────────────────────────────┐
       │   Object Crop Generator    │
       └───────────┬───────────────┘
                   │ Cropped Patches
                   ▼
       ┌────────────────────────────┐
       │    Vision–Language Model   │
       │  (LLaVA / BLIP / Florence) │
       └───────────┬───────────────┘
                   │ Text Metadata
                   ▼
       ┌────────────────────────────┐
       │  Annotation Merger + JSON  │
       └────────────────────────────┘

📂 Dataset

This project uses the DOTA v1.0 / v1.5 / v2.0 dataset.

Aerial images (large-size, multi-angle, multi-scale)

15 categories (airplane, ship, vehicle, tennis court, etc.)

Oriented bounding box format

Dataset link:
https://captain-whu.github.io/DOTA/dataset.html

⚙️ Installation
git clone https://github.com/<user>/DOTA-VLM.git
cd DOTA-VLM

conda create -n dota_vlm python=3.10 -y
conda activate dota_vlm

pip install -r requirements.txt


Dependencies include:

PyTorch

Transformers

timm

opencv-python

shapely

ultralytics (optional for YOLO-OBB)

🚀 Usage
1. Run Oriented Object Detector
python detection/run_detector.py \
    --input_dir data/DOTA/images \
    --output detections.json \
    --model_path checkpoints/yolo_obb.pt

2. Crop Objects
python tools/crop_objects.py \
    --detections detections.json \
    --images_dir data/DOTA/images \
    --out_dir crops/

3. Generate VLM Annotations
python vlm/generate_annotations.py \
    --crops_dir crops/ \
    --model llava \
    --output metadata.json

4. Export DOTA-VLM JSON
python tools/merge_annotations.py \
    --dota_json detections.json \
    --vlm_json metadata.json \
    --output dota_vlm.json

📝 Annotation Types

The VLM adds:

✔ Object-Level Attributes

Size

Shape

Appearance descriptors

Orientation

Activity (parked / moving / docked)

✔ Scene-Level Captions

Short and long descriptions of the aerial image.

✔ Spatial Relations

“next to”, “aligned with”, “clustered”, “parallel”, etc.

✔ Uncertainty

Ambiguous regions

Low-confidence object states
