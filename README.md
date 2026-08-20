# ChiroEcho: extending automated bat vocalisation classification beyond the learned taxonomy

![ChiroEcho architecture](figures/geographic_resolution_architecture.png)

**ChiroEcho** is a deep learning framework for automated classification of European bat echolocation calls. It jointly predicts species and genus from a shared acoustic encoder, and combines genus-level predictions with regional species distributions at inference time. When only one species of a predicted genus occurs at a recording's location, the framework resolves the prediction to that species — including species that were absent from the training taxonomy entirely.

Trained on 35 European bat species from [ChirosetEurope](https://doi.org/10.5281/zenodo.20773226), ChiroEcho extends operational coverage to 41 of the 48 native European bat species (73% → 85%), which to our knowledge is the broadest operational coverage reported for automated European bat classification to date.

This reframes geographic information as a means of *extending* a classifier's effective taxonomy, rather than merely constraining predictions within a fixed one.

**This repository is currently a placeholder.** Training and inference code will be added here in due time. In the meantime, this page describes the method and provides the citation for the associated workshop paper.

## Method overview

ChiroEcho uses a shared EfficientNet-B3 acoustic encoder with two linear heads — species (35 classes) and genus (11 classes) — trained jointly with a multi-label binary cross-entropy objective. The encoder can be initialized from ImageNet or from [Perch v2](https://www.kaggle.com/models/google/bird-vocalization-classifier) — a bioacoustic foundation model trained on birdsong. Since Perch v2's official releases are inference-only, we built [`perchv2-pytorch`](https://github.com/bghani/perchv2-pytorch), a PyTorch conversion of its backbone enabling deep fine-tuning; it was developed for this work and is documented separately. At inference, a lightweight rule-based post-processing step (`GeoResolveSpecies`) combines high-confidence genus predictions with a region–genus lookup table: when a genus has exactly one representative species in the recording's region, the genus prediction is resolved to that species, overriding the species head. This mechanism requires no additional training and can recover species entirely absent from the learned taxonomy, provided their genus is acoustically resolvable and they are the sole regional representative of that genus.

## Status

- [x] Paper accepted (CV4Ecology workshop, ECCV 2026)
- [ ] Training code — the Perch v2 PyTorch backbone used here is available now at [`perchv2-pytorch`](https://github.com/bghani/perchv2-pytorch); the ChiroEcho-specific dual-head training code (species + genus, geographic resolution) is still to come
- [ ] Inference / geographic resolution code
- [ ] Pretrained model weights

## Citing this work

If you use ChiroEcho or refer to this work, please cite:

```bibtex
@misc{ghani2026chiroechoextendingautomatedbat,
      title={ChiroEcho: extending automated bat vocalisation classification beyond the learned taxonomy}, 
      author={Burooj Ghani and Welmoed Eversteijn and Milan van Hirtum and Juan Sebastián Cañas    
      and Vincent J. Kalkman and Dan Stowell and A. Leonie Baier},
      year={2026},
      eprint={2608.18191},
      archivePrefix={arXiv},
      primaryClass={cs.LG},
      url={https://arxiv.org/abs/2608.18191},
      note={Accepted at the CV4Ecology Workshop, European Conference on Computer Vision (ECCV)},
}
```

## Related resources

- **Dataset**: [ChirosetEurope](https://doi.org/10.5281/zenodo.20773226) — a curated bioacoustics dataset of European bat vocalisations
- **Perch v2 PyTorch backbone**: [`perchv2-pytorch`](https://github.com/bghani/perchv2-pytorch) — unofficial PyTorch implementation of Perch v2
- **Affiliation**: [Naturalis Biodiversity Center](https://www.naturalis.nl/)

## About

No description, website, or topics provided.
