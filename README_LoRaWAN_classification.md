# LoRaWAN Classification and Forecasting for Network Diagnostics

A data-driven framework for **large-scale LoRaWAN network diagnostics**, combining:

- **protocol-aware feature engineering**
- **multi-layer unsupervised clustering**
- **probabilistic health scoring**
- **temporal forecasting of aggregated reliability indicators**

This repository accompanies the study on operator-oriented diagnostics for **massive IoT deployments over LoRaWAN**, with a focus on transforming large-scale telemetry into interpretable and actionable maintenance support.

## Overview

Large LoRaWAN deployments generate massive amounts of network-server telemetry, but the protocol itself exposes only limited direct diagnostics at device level. In real operational settings, this makes it difficult to distinguish among:

- radio-quality degradation
- protocol-side instability
- session resets or join-related anomalies
- congestion and infrastructure stress
- persistent device-side malfunctioning

This project addresses the problem through two complementary modules:

1. **Structural behavioral modeling**
   - device-level metrics are extracted from LoRaWAN traffic
   - features are organized into semantic layers
   - devices are clustered to identify latent operational regimes
   - a device-level health score is derived for reliability interpretation

2. **Temporal predictive monitoring**
   - aggregated group-level reliability indicators are built over time
   - a forecasting model predicts the temporal evolution of reliability trends
   - operators can monitor expected degradation, stability, or recovery at hotspot level

---

## Repository structure

The repository is organized around two main workflows.

```text
LoRaWAN_classification/
│
├── clustering/
│   └── [clustering script or notebook]
│
├── forecasting/
│   └── [TFT script or notebook]
│
├── data/                       # not included in the public repository
├── outputs/                    # generated figures, tables, intermediate artifacts
├── README.md
└── requirements.txt            # optional / if provided
```

If the repository uses different filenames, replace the placeholders below with the actual file names.

### Main components

#### 1. Clustering module
This component implements the **device-level diagnostic pipeline**.

Typical responsibilities:
- parse LoRaWAN telemetry exported from the network server
- compute device-level metrics over a fixed observation window
- organize descriptors into semantic layers
- cluster devices independently by layer
- combine layer-wise structural information
- derive an interpretable health score and reliability classes
- generate maps, distributions, and diagnostic plots

#### 2. Forecasting module
This component implements the **predictive monitoring pipeline**.

Typical responsibilities:
- aggregate device-level descriptors over time and by group
- select a reliability indicator as target, typically `PLR_dw`
- use the remaining aggregated descriptors as covariates
- train a Temporal Fusion Transformer (TFT) or equivalent multivariate forecasting model
- forecast future reliability trends across hotspot groups
- support static vs predictive comparison of network-health configurations

---

## Methodological summary

### 1. Device-level feature engineering

The framework extracts protocol-aware metrics from LoRaWAN telemetry and groups them into semantic layers.

#### Physical layer
Examples:
- RSSI mean / standard deviation
- SNR mean / standard deviation
- average number of gateways receiving the uplinks
- local spatial density or congestion proxy

These descriptors capture link quality, channel variability, and reception redundancy.

#### Data layer
Examples:
- error ratio
- data-rate switching ratio
- FPort consistency

These descriptors capture instability in signaling dynamics and application-level transmission coherence.

#### Network layer
Examples:
- active-days ratio
- payload-empty ratio
- frame-counter reset ratio

These descriptors capture temporal continuity and long-term operational regularity.

#### Protocol layer
Examples:
- downlink load occupancy
- packet loss ratio
- join-event ratio

These descriptors capture control-plane pressure, inferred loss, and session instability.

---

### 2. Multi-layer clustering

Rather than clustering the full feature space directly, the project adopts a **layer-wise clustering strategy**.

Why:
- monolithic clustering tends to mix unrelated effects
- it may generate clusters that are hard to interpret operationally
- layer-specific grouping preserves semantic structure

The layer-wise outputs are then combined to derive **behavioral regimes** and support **health-score estimation**.

Typical outputs:
- cluster assignments or soft memberships
- layer-wise behavioral profiles
- reliability classes
- spatial distributions of degradation

---

### 3. Group-level aggregation

Once device-level descriptors are available, they are aggregated at a higher level for operational analysis.

Possible aggregation units include:
- spatial cells
- hotspot groups
- gateway-based groups
- municipality or administrative areas
- infrastructure-oriented partitions

A key target used in the predictive stage is typically:

- **`PLR_dw`**: device-weighted packet loss ratio at group level

This provides an interpretable proxy of reliability degradation and is coherent with maintenance-oriented analysis.

---

### 4. Forecasting

The predictive component models the temporal evolution of aggregated reliability indicators.

#### Why TFT
The adopted forecasting model is suitable because:
- the evolution of reliability is **not purely univariate**
- the future trend of `PLR_dw` depends on the coupled evolution of other aggregated descriptors
- the model can use **dynamic covariates**
- the architecture supports **multi-horizon forecasting**
- the variable selection mechanism improves interpretability

In other words, `PLR_dw` is not forecast as an isolated sequence, but as part of a **multivariate temporal process** driven by the other behavioral indicators.

Typical setup:
- daily granularity
- fixed encoder length / lookback window
- multi-step horizon
- chronological train / validation / test split
- no recomputation of structural groups on the test horizon

---

## Expected inputs

The code is designed for **LoRaWAN network-server telemetry** and associated metadata.

Typical required information:
- device identifier (`DevEui`)
- timestamps
- frame counter (`FCnt`)
- RSSI / SNR
- gateway information
- protocol / MAC-side metadata
- error flags
- optional spatial coordinates
- optional device registry / asset information

Depending on the scripts, additional files may include:
- device registry CSV
- gateway locations JSON
- exported telemetry in JSONL or CSV format
- precomputed aggregation files

---

## Outputs

Typical outputs of the repository include:

### From the clustering pipeline
- device-level feature tables
- cluster assignments
- health scores
- reliability classes
- spatial hotspot maps
- feature-distribution figures
- tables for paper-ready summaries

### From the forecasting pipeline
- multivariate aggregated time series
- train / validation / test splits
- forecasting metrics
- forecast curves for representative hotspot groups
- static vs predictive hotspot comparisons
- ablation or sensitivity summaries

---

## How to use

### A. Clustering workflow
Run the clustering file or notebook to:
1. ingest raw telemetry
2. compute device-level descriptors
3. perform layer-wise clustering
4. derive reliability regimes and health scores
5. export figures and summary tables

Example placeholder:

```bash
python clustering/[your_clustering_file].py
```

or open the clustering notebook and execute the cells sequentially.

### B. Forecasting workflow
Run the forecasting file or notebook to:
1. build group-level aggregated time series
2. define forecasting target and covariates
3. train the TFT model
4. evaluate forecasting performance
5. generate prediction figures and error summaries

Example placeholder:

```bash
python forecasting/[your_tft_file].py
```

or use the corresponding notebook.

---

## Reproducibility notes

To reproduce the results reported in the paper, the following aspects must be kept consistent:

- same observation windows for descriptor computation
- same grouping strategy for hotspot or sub-network aggregation
- same chronological train / validation / test split
- same preprocessing and normalization
- same target definition for `PLR_dw`
- same set of covariates
- same clustering configuration per semantic layer

Because the study relies on real industrial telemetry, exact replication also depends on access to the original operational exports.

---

## Data availability and GDPR note

The raw operational datasets used in this project are **not included in this public repository**.

They originate from a real industrial LoRaWAN deployment and contain telemetry and metadata derived from operational network-server activity. For privacy, confidentiality, and contractual reasons, these data cannot be redistributed publicly.

### GDPR / privacy statement
The public repository is designed **not to expose personal data** or customer-identifying information. In particular:

- raw telemetry exports are excluded from version control
- customer-specific datasets are not distributed
- any public material should be anonymized or aggregated before release
- identifiers, coordinates, or metadata that could enable re-identification should be handled according to the applicable legal and contractual constraints

The repository therefore only contains:
- code
- documentation
- optionally synthetic, anonymized, or derived artifacts that do not disclose sensitive operational information

Anyone reusing this code on proprietary or real deployment data is responsible for ensuring compliance with:
- GDPR and other applicable privacy regulations
- internal data-governance policies
- industrial confidentiality and data-sharing agreements

---

## Adaptability beyond LoRaWAN

Although this repository focuses on LoRaWAN, the overall framework is largely transferable to other **LPWAN** technologies and, more generally, to **massive narrowband IoT scenarios**.

What remains reusable:
- aggregation strategy
- multi-layer behavioral modeling logic
- health-score and regime interpretation
- forecasting of reliability indicators

What requires adaptation:
- protocol-aware feature engineering
- session-management descriptors
- technology-specific signaling metrics
- loss inference logic
- control-plane load descriptors

In alternative communication settings, the LoRaWAN-specific observables should be replaced by corresponding technology-dependent indicators while preserving the same data-driven diagnostic rationale.

---

## Recommended citation

If you use this repository in academic work, please cite the corresponding paper associated with this project.

A repository-level citation entry can be added here once the final bibliographic details are available.

---

## Contact

For questions related to the methodology or academic use of the repository, please contact the repository maintainers.

Repository URL:
`https://github.com/netlab-sapienza/LoRaWAN_classification`
