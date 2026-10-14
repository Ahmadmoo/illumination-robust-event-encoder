# Illumination-Robust Event Encoder

Research project for learning event-camera representations that remain useful across lighting conditions through paired, physics-informed self-supervised learning.

## Goal

Train an encoder on corresponding observations of the same scene under different sensor illumination levels. Encourage consistent scene and motion features while accounting for illumination-dependent noise, latency, missing events, and sensor differences. At inference, use events from a single camera.

The proposed capture setup uses two identical, synchronized event cameras viewing the same scene through a beam splitter, with an ND filter on one optical path. Existing datasets will be investigated before deciding whether new recordings are needed.

## Status

Project scaffold only. Dataset loaders, model, losses, and training/evaluation scripts have not been implemented. No datasets or model weights are included.

## Dataset folders

| Folder | Dataset | Capture notes |
| --- | --- | --- |
| `datasets/LLE-VOS/` | [LLE-VOS](https://github.com/HebeiFast/EventLowLightVOS) | Two DAVIS346 cameras, a beam splitter, geometric calibration, and temporal synchronization. The main paper describes changing APS frame exposure times; physical illumination differences between the event sensors remain unverified. |
| `datasets/SDE/` | [SDE / EvLight](https://github.com/EthanLiang99/EvLight) | A DAVIS346 records repeated robot trajectories with and without an ND8 filter. The paired recordings are separate passes, so temporal alignment and scene dynamics need checking. |

Both folders contain only an empty `.gitkeep` placeholder. Downloaded dataset contents are ignored by Git. Follow each dataset's access and usage terms.

### Source papers

- LLE-VOS: [Event-assisted Low-Light Video Object Segmentation, CVPR 2024](https://arxiv.org/abs/2404.01945), Section 3.2.
- SDE: [Towards Robust Event-guided Low-Light Image Enhancement, CVPR 2024](https://openaccess.thecvf.com/content/CVPR2024/papers/Liang_Towards_Robust_Event-guided_Low-Light_Image_Enhancement_A_Large-Scale_Real-World_Event-Image_CVPR_2024_paper.pdf).
- SDE capture details: [EvLight++](https://arxiv.org/abs/2408.16254).

## First experiment

1. Inspect the released data: availability of both raw event streams, illumination changes, timestamps, calibration, sensor settings, and sequence splits. Shorter APS exposure alone does not establish reduced illumination at the event sensor.
2. Visualize aligned event windows from a few corresponding sequences and measure event rate, sparsity, and alignment error.
3. Establish a simple encoder baseline using the same event representation and augmentations as the paired-learning model.
4. Compare ordinary SSL against learning from verified cross-illumination pairs, using the same architecture and training budget. Use scene-level splits and keep paired views in the same split.
5. Evaluate representation consistency and a downstream task with a frozen encoder or a fixed fine-tuning budget. Report performance by lighting condition and on held-out scenes or datasets.

Dataset suitability and the exact physics-informed objective are open research decisions. The central hypothesis is that verified cross-illumination training improves downstream generalization; this scaffold contains no experimental results.
