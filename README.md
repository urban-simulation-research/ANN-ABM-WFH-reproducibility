# ANN-ABM-WFH-reproducibility

This repository provides a minimal reproducible description of the hybrid ANN–ABM framework used to simulate urban expansion under differentiated work modalities (WFH vs. WOS) in the Roanoke region.

## Purpose
The materials in this repository are intended to support review-stage transparency by documenting the model workflow, key decision rules, and simulation parameters in an anonymized form. The full codebase and complete data package will be released upon acceptance.

## Repository contents
- `pseudo_code.txt`  
  Step-by-step pseudo-code describing the simulation workflow, including data preprocessing, agent construction, ANN classification, LCS computation, annual allocation, and map updating.

- `config_parameters.txt`  
  Summary of the main simulation settings and decision parameters, including:
  - grid resolution
  - simulation periods
  - gamma (`γ = 0.5`)
  - operational 0.5 cutoffs
  - annual quota rule
  - high-confidence decision rule (no separate fixed `τ`)
  - WFH/WOS agent construction

## Model overview
The model integrates:
- Artificial Neural Networks (ANNs) for:
  - LULC transition prediction (`f_LULC`)
  - WFH residential preference estimation (`f_WFH`)
  - WOS residential preference estimation (`f_WOS`)
  - structural land adjustability estimation (`f_isAdjustable`)
- Agent-Based Modeling (ABM) for behavioral decision logic
- GIS-based spatial preprocessing and constraint handling
- CA-style neighborhood context through local LULC composition metrics

## Key modeling logic
- WFH and WOS are represented probabilistically at the cell level
- WFH probabilities are derived from:
  - ACS worked-at-home rates
  - OnTheMap worker home-location data
  - NAICS-based WFH feasibility scores, adjusted by wages
- The LULC Changeability Score (LCS) is computed as:

  `LCS = (1 - γ) * S_pbw + γ * P_isAdjustable`

- In this study:
  - `γ = 0.5`
  - agent-score screening threshold = `0.5`
  - LCS transformation threshold = `0.5`
  - no separate fixed high-confidence threshold `τ` is used
  - high-confidence predictions follow the class with the highest ANN probability

## Annual quota rule
Newly developed cells are allocated annually according to a historical growth constraint. The annual quota is derived from the observed average annual urban expansion rate during 2011–2019. Eligible cells are ranked by LCS and selected until the quota is reached.

## Reproducibility note
These files provide a compact description of the workflow and configuration needed to understand and independently follow the simulation logic during peer review. They are not intended to replace the full implementation.

## Data and code availability
The study uses publicly available sources including NLCD, ACS, OnTheMap, zoning, and conservation data. The complete implementation and full processed datasets will be made publicly available upon acceptance.
