# BIZ-1 | Agents Plan — Hyper-Local Opportunity & Temporal Reliability

**Contributor:** Manan  
**Branch:** BIZ-1-geospatial-opportunity  
**Modules:** 1 (Hyper-Local Opportunity Intelligence) + 2 (Temporal-Aware Feasibility)

---

## Module 1 — Hyper-Local Business Opportunity Intelligence

### Goal
Produce a multi-dimensional opportunity profile for a specific business location using publicly available geospatial, demographic and POI data.

### Agent Steps
1. **Location Ingestion** — Accept lat/lon + business type from user profile
2. **Spatial Feature Extraction** — Query OSMnx/PostGIS for competitors, POIs, road network, population
3. **Feature Engineering** — Buffer/radius analysis, KDE, H3 grid aggregation
4. **Model Inference** — Run XGBoost / LightGBM on engineered features
5. **SHAP Explanation** — Generate factor-level explanations for opportunity score
6. **Profile Output** — Return demand potential, competition pressure, accessibility, confidence

### Inputs
| Field | Source |
|-------|--------|
| Latitude, Longitude | User input |
| Business type | User input |
| Competitor locations | OpenStreetMap / OSMnx |
| Population density | Census / demographic dataset |
| POIs | OpenStreetMap |
| Road network | OSMnx |

### Outputs
```json
{
  "opportunity_score": 0.74,
  "opportunity_category": "High",
  "demand_potential": "Strong",
  "competition_pressure": "Moderate",
  "accessibility": "Good",
  "data_confidence": "Medium",
  "shap_explanation": { "population_density": 0.21, "competitor_count": -0.14 }
}
```

---

## Module 2 — Temporal-Aware Feasibility

### Goal
Quantify how much confidence should be placed in the opportunity assessment given that different data sources were collected at different times.

### Agent Steps
1. **Metadata Retrieval** — Fetch source, collection date, update frequency per feature
2. **Freshness Decay** — Apply exponential decay: `score = exp(-λ × days_old)`
3. **Drift Detection** — Run PSI / KS test to detect distribution shift
4. **Confidence Aggregation** — Weight opportunity score by composite freshness
5. **Revalidation Flag** — Trigger alert if confidence falls below threshold

### Decay Rate Guidelines
| Feature Type | Suggested λ (decay rate) |
|---|---|
| Competitor count | 0.05 (fast decay) |
| Road network | 0.005 (slow decay) |
| Population data | 0.01 (medium decay) |
| Market prices | 0.08 (very fast decay) |

### Outputs
```json
{
  "overall_confidence": "Medium",
  "competitor_data_age_days": 180,
  "freshness_score": 0.60,
  "drift_detected": false,
  "recommendation": "Revalidate competitor data before final investment decision"
}
```

---

## Open Tasks
- [ ] Calibrate decay rates using historical data comparison
- [ ] Integrate ADWIN for streaming drift detection
- [ ] Run temporal experiments: fresh vs 3/6/12/18-month-old data
- [ ] Connect Module 2 confidence output to Module 7 explainability layer
