---
citation_count: 63
citation_source: "Semantic Scholar, 2026-10-15 (value carried over from file; live API re-check returned HTTP 429)"
venue: "ICLR 2019"
date: 2019-03-12
research_group: "Stanford (Jiajun Wu)"
---

# Unsupervised Discovery of Parts, Structure, and Dynamics (PSD)

- **Authors:** Zhenjia Xu*, Zhijian Liu*, Chen Sun, Kevin Murphy, William T. Freeman, Joshua B. Tenenbaum, **Jiajun Wu**. (*equal contribution)
- **Group role:** senior (last) author and corresponding author (per the paper's correspondence footnote). This is MIT-era work, before Wu joined Stanford.
- **Date published:** 2019-05 (ICLR 2019 conference, May 2019; arXiv 2019-03-12)
- **Venue:** ICLR 2019. The PDF header reads "Published as a conference paper at ICLR 2019". Presentation type not verified.
- **Links:**
  - arXiv: https://arxiv.org/abs/1903.05136
  - Project: http://psd.csail.mit.edu
  - Code: https://github.com/zhenjia-xu/psd
- **Number of citations:** 63 (source: Semantic Scholar API, checked 2026-10-15)

## Summary
PSD is a stochastic, passive (not action-conditioned) video world model. From a single frame it predicts possible next frames. It also discovers, without supervision:
- object parts, as a layered image representation;
- their hierarchical tree structure, through a learned "structural descriptor" that composes local motions into global ones.

The model is a conditional VAE built on cross-convolution motion prediction (Visual Dynamics). It is trained on pairs of frames.

## 1. Weights availability
Released. The repo README states it "contains pre-trained models and testing code". Run `./download_model.sh` to get the pretrained model (185 MB), then `demo.py`. No license is declared.

## 2. Code
- Repository: https://github.com/zhenjia-xu/psd
- Contents: testing and demo code, a generator for the shapes dataset, and the pretrained-model download script.
- I did not confirm whether training code is included; the README documents only testing.
- Last commit: 2019-11-26. No LICENSE file.

## 3. Datasets
- **Shapes and digits (synthetic):** 100K training pairs and 10K test pairs each.
  - Digits are MNIST.
  - There is an extra 9-shape set.
  - The data can be generated with the repo script.
- **Atari Basketball:** 5K training pairs and 500 test pairs, from a YouTube video.
- **Human exercise (from Xue et al.):** 50K training pairs and 500 test pairs.
- **Yoga (from Balakrishnan et al.):** 4,720 training pairs.
- **Optical flow:** precomputed with Liu (2009).

## 4. Evaluation and downstream tasks
- **Tasks evaluated:**
  - future-frame synthesis (multiple sampled futures);
  - object/part segmentation (IoU);
  - recovery of the hierarchical structure.
- **Segmentation:** PSD outperforms NEM and R-NEM on shapes and digits (Table 1) and on human exercise parts (Table 2). In Table 2, the last row as extracted reads 0.251 / 0.276 / 0.474 for NEM / R-NEM / PSD. The column-to-method mapping comes from PDF text extraction and is not certain.
- **Structure discovery:** PSD recovers the correct trees where NRI and R-NEM fail (qualitative).
- **Future prediction:** fewer artifacts than 3DcVAE (qualitative, Fig. 12).
- **Downstream tasks:** none (no planning or control).

## 5. Architecture and techniques
- **Overall design:** a conditional VAE with d = 32 latent dimensions.
  - **Motion encoder** (training only): a 7-layer CNN over the optical flow, producing z.
  - **Kernel decoders:** one per latent dimension, each outputting a 5×5 kernel.
  - **Image encoder:** a 6-layer CNN producing a 64-channel feature map at 128×128.
  - **Cross-convolution:** applies the kernels to the feature map, channel-wise.
  - **Motion decoder:** depthwise-separable convolutions that predict per-part local motion.
- **Structural descriptor:** a learned relaxed ancestor matrix S that maps local motions to global motions.
- **Image decoder:** a U-Net that warps frame 1 into frame 2.
- **Loss:** reconstruction (flow plus image) + β·KL + γ·L1 penalty on local motion.
- **Training:** two stages. First, disentangled representations, with β doubled adaptively. Second, structure learning.

## 6. Conclusions and future work
- **Authors' conclusions:** parts, their hierarchy and their dynamics can be learned jointly from unlabeled video pairs. The model handles both synthetic and real data.
- **Own observations:**
  - It is a single-step, passive predictor at low resolution (128×128).
  - Motion comes from precomputed optical flow during training.
  - There are no action inputs.
  - It is a borderline "world model". It is included as a Wu-last, top-venue, unsupervised dynamics and future-prediction paper.

## 7. Sources and verification
- Read: arXiv PDF https://arxiv.org/pdf/1903.05136, Secs. 1–6.
- Read: repo README at https://github.com/zhenjia-xu/psd. The repo is linked from the project page.
- Venue: Semantic Scholar batch API returned ICLR.
- **Not verified:** the exact IoU table layout (multi-column PDF extraction).
