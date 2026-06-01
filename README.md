# LoRaWAN Classification

Code repository for a data-driven framework for **LoRaWAN network diagnostics** in **massive IoT deployments**.

The project combines:
- **protocol-aware feature engineering**
- **multi-layer unsupervised clustering**
- **probabilistic health scoring**
- **forecasting of aggregated reliability indicators**

## Repository contents

The repository includes two main components:

- **Clustering module**  
  Computes device-level descriptors from LoRaWAN telemetry, organizes them into semantic layers, performs clustering, and derives interpretable reliability regimes and health scores.

- **Forecasting module**  
  Builds aggregated time series over device groups / hotspot areas and predicts the evolution of reliability indicators, with particular focus on `PLR_dw`.

## Main idea

The framework is designed to help operators analyze large-scale LoRaWAN deployments where direct device-level inspection is impractical.

The workflow is:
1. extract protocol-aware metrics from telemetry
2. group them into semantic layers
3. identify latent behavioral regimes through clustering
4. derive device- or group-level reliability indicators
5. forecast their temporal evolution for predictive monitoring

## Data

The raw operational data used in the associated study are **not included** in this repository.

They come from a real industrial deployment and cannot be publicly released for:
- privacy reasons
- confidentiality constraints
- industrial data-sharing restrictions

### GDPR / privacy note
This repository is intended to contain only:
- code
- documentation
- non-sensitive derived material

No raw customer data or personally identifiable information should be uploaded.  
Anyone reusing the code on proprietary datasets is responsible for ensuring compliance with GDPR, internal governance rules, and any applicable confidentiality agreements.

## Portability

Although the current implementation focuses on **LoRaWAN**, the overall methodology is largely transferable to other **LPWAN** or **narrowband IoT** scenarios.

The main part that would require adaptation is the **feature engineering stage**, especially for technology-specific descriptors related to signaling, session management, packet-loss inference, and control-plane behavior.

## Usage

Run the two main scripts / notebooks separately:

- **clustering file**: structural behavioral modeling and health scoring
- **TFT file**: temporal forecasting of aggregated indicators

Update the input paths according to your local dataset organization before execution.

## Citation

If you use this repository in academic work, please cite the corresponding paper associated with this project.

Repository: to be added.

