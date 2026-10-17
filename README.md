# BizNiti

BizNiti is a prototype for helping entrepreneurs assess small-business opportunities and understand the financial feasibility of starting a business. The project is organized as modular work: the notebooks in this repository explore local-market opportunity analysis and business financing, with government-scheme intelligence planned as a connected capability.

> **Project status:** This is a research/prototyping repository, not a production application. Several planned features are currently placeholders. Outputs should not be used as investment, lending, or eligibility advice.

## What is in this repository

| File | Purpose |
| --- | --- |
| [`manan_01_geospatial_opportunity.ipynb`](./manan_01_geospatial_opportunity.ipynb) | Geospatial opportunity-analysis prototype: sample location and catchment inputs, a feature-extraction stub, and a placeholder opportunity profile. |
| [`manthan_01_financial_feasibility.ipynb`](./manthan_01_financial_feasibility.ipynb) | Financial-feasibility prototype: funding gap, EMI, cash flow, break-even revenue, DSCR, and Monte Carlo cash-flow simulation. Capital optimization remains a stub. |
| [`manan_agents_plan.md`](./manan_agents_plan.md) | Plans for hyper-local opportunity intelligence and temporal data reliability. |
| [`manan_case_format.json`](./manan_case_format.json) | Draft input, feature, temporal-metadata, and output schema for the geospatial module. |
| [`manthan_agents_plan.md`](./manthan_agents_plan.md) | Plans for financial feasibility, capital structuring, and government-scheme intelligence. |
| [`manthan_case_format.json`](./manthan_case_format.json) | Draft input and output schema for financial analysis and scheme matching. |

## Current capabilities and limitations

### Geospatial opportunity notebook

The notebook uses a sample location and business type to demonstrate the intended flow. Feature extraction currently returns placeholder values; it does **not** yet query OpenStreetMap, population datasets, a spatial database, or an H3 index. Opportunity scores and SHAP explanations are also pending model and data integration.

### Financial feasibility notebook

The notebook currently calculates:

- Funding gap after own capital and any entered subsidy
- Monthly loan EMI
- Net operating income and monthly cash flow
- Break-even revenue and debt service coverage ratio (DSCR)
- A 10,000-run cash-flow simulation using an assumed revenue distribution

The simulation uses an illustrative volatility assumption, not business-specific historical data. PuLP-based capital-structure optimization, historical revenue forecasting, and government-scheme retrieval/eligibility checks are planned but not implemented.

## Getting started

### Requirements

- Python 3
- Jupyter Notebook support (JupyterLab, classic Jupyter, or the VS Code Jupyter extension)
- `numpy` and `pandas`

There is no dependency or environment manifest in this repository yet. Install the current notebook dependencies in your chosen Python environment:

```bash
python -m pip install numpy pandas notebook
```

### Run the notebooks

From the `BizNiti` directory, start Jupyter:

```bash
jupyter notebook
```

Open either notebook and run its cells from top to bottom. In VS Code, open a notebook directly, select a Python kernel with the dependencies installed, and run all cells.

The geospatial notebook's location, catchment radius, and business type are configured in its **Configuration** section. The financial notebook's example entrepreneur inputs are in its **Financial Inputs** section. Change these values to explore the implemented calculations; geospatial results will remain placeholders until data extraction and model inference are added.

## Module plans

The repository currently contains work related to these planned modules:

1. **Hyper-local business opportunity intelligence:** estimate demand, competition, points of interest, and accessibility for a proposed business location.
2. **Temporal reliability:** track source freshness, data drift, and when an assessment should be revalidated.
3. **Financial feasibility and capital structuring:** evaluate cash flow, repayment capacity, and funding needs.
4. **Government-scheme intelligence:** retrieve relevant schemes, check documented eligibility criteria, and assess financial compatibility.

See the agent plans and case-format JSON files for proposed inputs, outputs, and outstanding work. These documents describe intended designs and should not be interpreted as implemented functionality.

## Suggested next steps

- Connect the geospatial notebook to real, appropriately licensed data sources and implement feature extraction.
- Establish labelled data and a baseline model before producing opportunity scores or explanations.
- Validate financial formulas and simulation assumptions against representative cases.
- Implement and test capital-structure optimization and scheme eligibility against authoritative, current scheme sources.
- Add a dependency manifest, automated tests, and clear data-source/version metadata as the prototype matures.

## Data and responsible use

Location, demographic, competitor, and policy data can be incomplete, outdated, or unavailable for a particular area. The proposed temporal-confidence fields are part of the design; they are not yet calculated by the geospatial notebook. Verify data, scheme conditions, loan terms, and all financial assumptions with authoritative sources and qualified professionals before making business or financing decisions.