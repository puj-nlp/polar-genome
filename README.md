# POLAR-Genome: Multilingual Polarization Detection with Hybrid Bio-Inspired Representations

[![License](https://img.shields.io/github/license/puj-nlp/polar-genome)](LICENSE)
[![Python](https://img.shields.io/badge/python-3.10-blue)](https://www.python.org/)
[![Institution](https://img.shields.io/badge/institution-Pontificia%20Universidad%20Javeriana-red)](https://www.javeriana.edu.co)

Supplementary materials for the paper:

> **Quantitative Properties of Polarized Discourse: A Multilingual Distributional Analysis of Lexical and Structural Patterns in English and Spanish Social Media Texts**  
> Juan Sebastián Parra Sandoval · Luis Gabriel Moreno Sandoval  
> Pontificia Universidad Javeriana, Bogotá, Colombia  
> *Submitted to Journal of Quantitative Linguistics, 2026*

---

## Repository Structure
```
polar-genome/
├── Prototipo.ipynb                    # Main pipeline: embeddings, k-mers, classification
├── Análisis_Datos_Tésis.ipynb         # Exploratory data analysis (thesis)
├── Análisis_de_Datos_(Unified).ipynb  # Quantitative linguistic analysis (unified corpus)
├── Unified_Dataset_Clean_Final.csv    # Final unified corpus (5,237 instances, EN/ES)
├── Merged_Dataset.csv                 # Intermediate merged dataset
├── Merged_Labeled.csv                 # Artificially labeled dataset
├── Trial_Data.csv                     # SemEval 2026 Task 9 POLAR trial data (gold labels)
├── english_dataset.csv                # HASOC 2019 English subset
├── toxic_test.csv                     # Spanish toxic content subset
└── LICENSE
```

## Dataset

The unified corpus (`Unified_Dataset_Clean_Final.csv`) contains **5,237 instances** in English (n = 4,102) and Spanish (n = 1,135), assembled from three sources:

| Source | Language | Instances | Annotation |
|---|---|---|---|
| SemEval 2026 Task 9 – POLAR (Trial) | EN / ES | ~66 | Gold standard |
| HASOC 2019 (Mandl et al., 2019) | English | ~4,036 | Artificial (Moreno-Sandoval et al., 2024) |
| Toxic Test – Spanish social media | Spanish | ~1,135 | Artificial (Moreno-Sandoval et al., 2024) |

**Schema:** `text`, `lang`, `id`, `political`, `racial/ethnic`, `religious`, `gender/sexual`, `vilification`, `dehumanization`, `extreme_language`, `lack_of_empathy`, `stereotype`, `invalidation`, `other`, `polarization`

## Reproducibility

| Component | Version |
|---|---|
| Python | 3.10 |
| sentence-transformers | 2.2.2 |
| scikit-learn | 1.3.0 |
| lightgbm | 4.1.0 |
| scipy | 1.11.3 |
| imbalanced-learn | 0.11.0 |
| nltk | 3.8.1 |
| spacy | 3.x + en_core_web_sm + es_core_news_sm |
| textstat | 0.7.13 |

Random seed: `RANDOM_STATE = 42`  
Hardware: NVIDIA T4 GPU (16 GB VRAM)

## Pipeline Overview

1. **Preprocessing** — tokenization, lowercasing, noise removal, language tagging
2. **Dual Representation**
   - *LLMeval*: DistilUSE-base-multilingual-cased-v2 → 512-dim embeddings
   - *Genome-kmers*: sliding window k=3 over tokens → TF-IDF (max 5,000 features)
3. **Fusion** — sparse horizontal concatenation (`scipy.hstack`)
4. **Classification** — LightGBM / Logistic Regression / LinearSVM with 5-fold CV

## Results Summary

| Subtask | Best Config | F1-macro | 95% CI |
|---|---|---|---|
| Subtask 1 – Binary Detection | POLAR+Genome / LightGBM | 0.778 ± 0.020 | [0.760, 0.796] |
| Subtask 2 – Target Classification | POLAR+Genome / SVM | 0.608 ± 0.055 | [0.560, 0.656] |
| Subtask 3 – Manifestation ID | POLAR+Genome / SVM | 0.346 ± 0.095 | [0.263, 0.430] |

## Citation

If you use this code or data, please cite:
```bibtex
@article{parra2026polar,
  title     = {Quantitative Properties of Polarized Discourse: A Multilingual 
               Distributional Analysis of Lexical and Structural Patterns 
               in English and Spanish Social Media Texts},
  author    = {Parra Sandoval, Juan Sebasti{\'{a}}n and 
               Moreno Sandoval, Luis Gabriel},
  journal   = {Journal of Quantitative Linguistics},
  year      = {2026},
  note      = {Under review}
}
```

## License

This project is licensed under the terms of the [LICENSE](LICENSE) file.  
The SemEval 2026 Trial Data is subject to the original task's terms of use. 
