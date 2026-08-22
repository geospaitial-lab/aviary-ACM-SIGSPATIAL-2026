<div align="center">

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://www.github.com/geospaitial-lab/aviary/raw/main/docs/assets/aviary_logo_white.svg">
  <img alt="aviary" src="https://www.github.com/geospaitial-lab/aviary/raw/main/docs/assets/aviary_logo_black.svg" width="30%">
</picture>

</div>

<div align="center">

[![Paper][Paper Badge]][Paper]
[![Repo][Repo Badge]][Repo]
[![Docs][Docs Badge]][Docs]

# Supplementary Materials

</div>

  [Paper Badge]: https://img.shields.io/badge/Paper-ACM%20SIGSPATIAL%202026-000000?logo=acm
  [Paper]: tbd
  [Repo Badge]: https://img.shields.io/github/v/release/geospaitial-lab/aviary?color=black&label=Repo&logo=GitHub
  [Repo]: https://github.com/geospaitial-lab/aviary
  [Docs Badge]: https://img.shields.io/github/actions/workflow/status/geospaitial-lab/aviary/docs.yaml?branch=main&color=black&label=Docs&logo=materialformkdocs&logoColor=white
  [Docs]: https://geospaitial-lab.github.io/aviary

This repository contains supplementary materials for the paper **aviary: A Modular Framework for ML Inference on Geospatial Data** (ACM SIGSPATIAL 2026).

---

## Application Scenarios

We demonstrate the applicability of the framework through four application scenarios, selected to cover a representative range of tasks, modalities, and ML backends across all three pipeline categories.
These scenarios utilize aerial imagery to monitor vegetation without the use of ML and create a dataset for training a segmentation model to detect building footprints, as well as analyze land cover using a segmentation model and detect vehicles using an object detection model.
The first two scenarios address general processing, while the latter two involve ML inference.

| Application Scenario                                  | Configuration File                            | Notebook                                           | Colab |
|-------------------------------------------------------|-----------------------------------------------|----------------------------------------------------| --- |
| [NDVI Calculation](application_scenarios/1/README.md) | [Config](application_scenarios/1/config.yaml) | [Notebook](application_scenarios/1/notebook.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/geospaitial-lab/aviary-ACM-SIGSPATIAL-2026/blob/main/application_scenarios/1/notebook.ipynb) |
| [Training Dataset Creation](application_scenarios/2/README.md) | [Config](application_scenarios/2/config.yaml) | [Notebook](application_scenarios/2/notebook.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/geospaitial-lab/aviary-ACM-SIGSPATIAL-2026/blob/main/application_scenarios/2/notebook.ipynb) |
| [Land Cover Segmentation](application_scenarios/3/README.md) | [Config](application_scenarios/3/config.yaml) | [Notebook](application_scenarios/3/notebook.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/geospaitial-lab/aviary-ACM-SIGSPATIAL-2026/blob/main/application_scenarios/3/notebook.ipynb) |
| [Vehicle Detection](application_scenarios/4/README.md) | [Config](application_scenarios/4/config.yaml) | [Notebook](application_scenarios/4/notebook.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/geospaitial-lab/aviary-ACM-SIGSPATIAL-2026/blob/main/application_scenarios/4/notebook.ipynb) |

---

## Additional Resources

- Paper: **tbd**
- Repository: [github.com/geospaitial-lab/aviary]
- Documentation: [geospaitial-lab.github.io/aviary]

  [github.com/geospaitial-lab/aviary]: https://github.com/geospaitial-lab/aviary
  [geospaitial-lab.github.io/aviary]: https://geospaitial-lab.github.io/aviary

---

## License

The supplementary materials are licensed under the [GPL-3.0 license].

  [GPL-3.0 license]: LICENSE.md
