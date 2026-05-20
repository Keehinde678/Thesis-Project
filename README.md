## A Computational Audit of Demographic Association Encoding ClinicalBERT'S Language Prediction

Overview
This repository contains the code and analysis pipeline for a thesis investigating how biases embedded in clinical documentation practices are transferred as representational harm in the predictions of ClinicalBERT. The study examines how demographic descriptors, race and gender, influence the model's predicted probabilities of behavioral and evaluative language commonly found in clinical notes.
The research follows a mixed-methods approach combining:

**Log Probability Bias Analysis (LPBA):** measuring how demographic markers shape ClinicalBERT's token predictions

**Probing Analysis (PA):** examining whether demographic information and evaluative language are structurally encoded in ClinicalBERT's internal representations

**Representational Harm Linguistic Analysis:** theory-driven interpretation grounded in Blodgett et al. (2020) and Crawford (2017)

**Research Context**
Clinical language models such as ClinicalBERT are increasingly deployed to analyse clinical notes, inform diagnoses, and support treatment recommendations. This study argues that because ClinicalBERT was pre-trained on MIMIC-III clinical notes. This is a corpus that reflects documented patterns of racial and gender bias in clinical documentation and the model inherits and reproduces those biases in its internal representations. The target words examined are grouped into two theoretically grounded categories:
Evaluative Framing: compliant, noncompliant
Behavioral Language: reliable, adherent, resistant, difficult
Agency attribution: active cooperation (refuses and agreed); active resistance(declined) and passive cooperation(presented)

**Methods**
1 **— Log Probability Bias Analysis (LPBA)**
LPBA measures the likelihood of specific target words appearing at a [MASK] position in controlled sentence templates when different demographic descriptors are present.
Design:

4 racial groups × 2 gender categories × 5 sentence templates × 6 target words = 240 observations
Templates follow the form: "The [race] [gender] patient appeared [MASK]."
Log probabilities extracted from ClinicalBERT's masked language model head using log-softmax over the full vocabulary
Outputs:

Raw log probabilities per demographic × word combination
Bias scores (deviation from grand mean log probability)
ANOVA and Kruskal-Wallis tests across racial groups
Independent samples t-test across gender groups
Visualisations: split heatmap, diverging dot plot, grouped bar charts by race and gender

**Method 2 — Probing Analysis (PA)**
PA examines whether demographic information and evaluative language are structurally encoded within ClinicalBERT's internal representations via two approaches:
MLM Template Probing:

Controlled templates of the form "The [race] [gender] patient appeared [MASK]"
4 racial groups × 2 gender categories × 5 templates = 480 observations
Captures predicted word probability distributions across demographic combinations

**Linear Probe Classifiers:
**
Trained on four linguistic feature types: unigrams, bigrams, trigrams, and clinical phrases
5-fold cross-validation
Decodes ethnicity, gender, and evaluative terms from learned representations
Evaluated by accuracy against majority baseline and Area Under the Curve (AUC)

**Method 3 — Representational Harm Linguistic Analysis**
Theory-driven qualitative interpretation of quantitative findings grounded in:

Blodgett et al. (2020): representational harm framework
Crawford (2017): distinction between representational and allocative harm
D'Ignazio and Klein (2020): data feminism and structural power in data systems

**Dataset**
This study uses MIMIC-III (Medical Information Mart for Intensive Care III), a large, freely available database of de-identified clinical notes from Beth Israel Deaconess Medical Center.

Access Requirements: MIMIC-III is not publicly downloadable without credentialing. To access the dataset you must:
Complete the CITI Data or Specimens Only Research training
Submit a data use agreement through PhysioNet
Receive credentialed access approval
This repository does include any MIMIC-III data but does contain no patient information.

**Model**
ClinicalBERT — emilyalsentzer/Bio_ClinicalBERT
A domain-specific BERT model further pre-trained on MIMIC-III clinical notes by Alsentzer et al. (2019). Available via 

HuggingFace:
https://huggingface.co/emilyalsentzer/Bio_ClinicalBERT

Installation
bash# Clone the repository
git clone https://github.com/keehinde678/clinicalbert-bias-analysis.git
cd clinicalbert-bias-analysis

# Install dependencies

pip install -r requirements.txt

requirements.txt

torch>=2.0.0

transformers>=4.30.0

pandas>=1.5.0

numpy>=1.24.0

matplotlib>=3.7.0

seaborn>=0.12.0

scipy>=1.10.0

scikit-learn>=1.2.0

Reproducing Results

Step 1: Run LPBA

bashcd lpba
python lpba_clinicalbert.py
ClinicalBERT (~440MB) will download automatically on first run. Results are saved to results/. Runtime is approximately 5–15 minutes on CPU.

Step 2: Run Probing Analysis

bashcd probing
python probing_analysis.py

Step 3: Interpreting outputs
Each script saves:

A raw results CSV with one row per observation

A bias metrics CSV with group-level scores and rankings

A statistical tests CSV with significance values

PNG visualisations split by word category (Evaluative Framing / Behavioral Language)


**Theoretical Framework**
This study adopts the representational harm framework (Blodgett et al., 2020; Crawford, 2017) as its primary interpretive lens. Representational harm refers to the ways in which language models encode and reproduce demeaning, reductive, or stereotyped associations about particular social groups, operating not by directly withholding resources but by perpetuating stigmatising associations that shape how groups are perceived and treated.
Statistical outputs from LPBA and PA are interpreted not as self-contained performance metrics but as evidence of how clinical language encodes and reproduces social hierarchies embedded in the documentation practices of the healthcare system.

Key References

Alsentzer, E. et al. (2019). Publicly Available Clinical BERT Embeddings.

Blodgett, S. L. et al. (2020). Language (Technology) is Power: A Critical Survey of "Bias" in NLP.

Crawford, K. (2017). The Trouble with Bias. NeurIPS Keynote.

D'Ignazio, C. & Klein, L. F. (2020). Data Feminism. MIT Press.

Johnson, A. E. W. et al. (2016). MIMIC-III, a freely accessible critical care database.

Kurita, K. et al. (2019). Measuring Bias in Contextualized Word Representations.

Mehrabi, N. et al. (2021). A Survey on Bias and Fairness in Machine Learning.

Obermeyer, Z. et al. (2019). Dissecting racial bias in an algorithm used to manage the health of populations.


**Citation**

If you use this code in your own research, please cite:

@thesis{Kehinde Soetan2026,

  title     = { A Computational Audit of Demographic Association Encoding ClinicalBERT'S Language Prediction},

  author    = {Kehinde Soetan},
 
  year      = {2026},
 
  school    = {The Ohio State University},

  type      = {Master's Thesis}
}

**License**

This project is licensed under the MIT License. See LICENSE for details.

**Contact**
For questions about this research, please open an issue or reach out via GitHub.
