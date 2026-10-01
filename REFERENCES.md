# Annotated primary references

[Home](README.md) · [Attribution](ATTRIBUTION.md)

This is a map of the antecedents needed to understand the cases, not a claim to
cover all SISR literature. A related paper supplies a mechanism or counterargument;
it does not validate our implementation or reproduce our hardware/quality results.

| Primary source | Why it matters here | Do not infer |
| --- | --- | --- |
| **Jiahui Yu et al., 2018.** [Wide Activation for Efficient and Accurate Image Super-Resolution](https://arxiv.org/abs/1808.08718), arXiv:1808.08718 | Wide activation as an efficiency/capacity design choice for WSR | That MACs predict our TensorRT latency |
| **Cheng Wan et al., 2023; revised 2024.** [Swift Parameter-free Attention Network for Efficient Super-Resolution](https://arxiv.org/abs/2311.12770), arXiv:2311.12770; [official code](https://github.com/hongyuanyu/SPAN) | SPAN baseline and Conv3XC structural reparameterization antecedent | That our context-gate arrangement or DUAL2 training dynamics were evaluated in SPAN |
| **Xintao Wang et al., 2018.** [Recovering Realistic Texture in Image Super-resolution by Deep Spatial Feature Transform](https://arxiv.org/abs/1804.02815), arXiv:1804.02815 | Conditional spatial feature modulation | That WSR uses the paper's semantic prior or training recipe |
| **Jingyun Liang et al., 2021.** [SwinIR: Image Restoration Using Swin Transformer](https://arxiv.org/abs/2108.10257), arXiv:2108.10257 | Shifted-window restoration transformer context | That GRAFT's border/position contracts are identical |
| **Xiangyu Chen et al., 2022 preprint.** [Activating More Pixels in Image Super-Resolution Transformer](https://arxiv.org/abs/2205.04437v1), arXiv:2205.04437v1 | HAT's hybrid and overlapping-attention motivation | That a headline system gain isolates one module or proves our design novel |
| **Yiqun Mei et al., 2020.** [Image Super-Resolution With Cross-Scale Non-Local Attention and Exhaustive Self-Exemplars Mining](https://openaccess.thecvf.com/content_CVPR_2020/html/Mei_Image_Super-Resolution_With_Cross-Scale_Non-Local_Attention_and_Exhaustive_Self-Exemplars_Mining_CVPR_2020_paper.html), CVPR 2020 | Internal cross-scale correspondence as evidence for reconstruction | That X/XG directly reproduce its full network |
| **Shangchen Zhou et al., 2020.** [Cross-Scale Internal Graph Neural Network for Image Super-Resolution](https://arxiv.org/abs/2006.16673), arXiv:2006.16673 | Cross-scale recurrence and transfer from internal exemplars | That nearest-neighbor graph retrieval has our soft-attention execution cost |
| **Syed Waqas Zamir et al., 2021 preprint; CVPR 2022.** [Restormer: Efficient Transformer for High-Resolution Image Restoration](https://arxiv.org/abs/2111.09881), arXiv:2111.09881 | Transposed/channel attention in restoration | That regional blending in DUAL is validated by Restormer |
| **Hang Wang et al., 2023.** [Omni Aggregation Networks for Lightweight Image Super-Resolution](https://openaccess.thecvf.com/content/CVPR2023/html/Wang_Omni_Aggregation_Networks_for_Lightweight_Image_Super-Resolution_CVPR_2023_paper.html), CVPR 2023 | Existing combination of spatial and channel interactions; important limit on novelty claims | That combining those two axes is itself a new primitive |
| **Yupeng Zhou et al., 2023.** [SRFormer: Permuted Self-Attention for Single Image Super-Resolution](https://arxiv.org/abs/2303.09735v1), arXiv:2303.09735v1 | Relevant prior art for spatial/channel rearrangement in efficient attention | That QPA's distinct shared-matching and narrowed-width choices preserve detail |
| **Yanqi Zhou et al., 2022.** [Mixture-of-Experts with Expert Choice Routing](https://arxiv.org/abs/2202.09368), arXiv:2202.09368 | Routing/allocation antecedent for planned efficiency research | That an expert-choice idea solves SISR scorer learnability or border/packing costs |

Versioned links matter: the linked SRFormer v1 is not the later revised V2 title;
the HAT debut link is the May 2022 version. We deliberately do not reproduce an
uncertain HAT ablation number from an unidentified version.

## What is project-specific?

The context-gate arrangement, XG's shared descriptors with independent-temperature
value routes, DUAL's regional blending and their TensorRT execution mapping are
the project-specific combinations investigated here. **Specific is not the same
as first-ever.** Novelty would require a broader, carefully scoped prior-art claim;
usefulness requires local evidence. Neither substitutes for the other.
