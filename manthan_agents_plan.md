# BIZ-3 | Agents Plan — Financial Feasibility & Government Scheme Intelligence

**Contributor:** Manthan  
**Branch:** BIZ-3-financial-schemes  
**Modules:** 3 (Financial Feasibility & Capital Structuring) + 4 (Government Scheme Intelligence)

---

## Module 3 — Financial Feasibility, Capital Structuring and Restructuring

### Goal
Determine whether the proposed business can be financially sustained and construct a feasible funding mix from own capital, loans and government support — with uncertainty quantified via scenario analysis.

### Agent Steps
1. **Input Collection** — Receive project cost, capital, revenue estimates and loan preferences from Module 5
2. **Deterministic Calculations** — Compute EMI, cash flow, break-even, funding gap, DSCR
3. **Financing Optimization** — Run MILP to find the best mix of own capital, loan and scheme support
4. **Scenario Simulation** — Monte Carlo over revenue and cost uncertainty
5. **Feasibility Output** — Return funding structure, repayment risk and confidence intervals
6. **API Exposure** — Serve as a FastAPI endpoint for other modules to call

### Inputs
| Field | Source |
|-------|--------|
| Project cost | User profile (Module 5) |
| Own capital / savings | User profile (Module 5) |
| Monthly revenue estimate | User input or business template |
| Fixed and variable costs | User input |
| Loan terms (rate, tenure) | Scheme database (Module 4) |
| Scheme subsidy | Module 4 eligibility output |

### Outputs
```json
{
  "funding_gap": 350000,
  "monthly_emi": 7441.50,
  "monthly_cashflow": 12558.50,
  "dscr": 1.68,
  "dscr_status": "Healthy",
  "breakeven_revenue": 35714.29,
  "prob_negative_cashflow": "4.2%",
  "recommended_structure": {
    "own_capital": 100000,
    "loan": 300000,
    "scheme_subsidy": 50000,
    "interest_cost_estimate": 89358
  }
}
```

---

## Module 4 — Government Scheme & Policy Intelligence

### Goal
Retrieve schemes relevant to the entrepreneur's profile, apply documented eligibility rules deterministically, and rank eligible schemes by financial compatibility.

### Agent Steps
1. **Query Construction** — Build structured query from user profile (business type, location, capital, age)
2. **Hybrid Retrieval** — BM25 keyword search + Sentence-Transformer dense retrieval
3. **Reranking** — Cross-encoder reranker on top-K candidates
4. **Eligibility Check** — Deterministic rule engine (age, sector, location, income, project cost)
5. **Financial Compatibility** — Match scheme loan limits and subsidy to Module 3 funding gap
6. **LLM Explanation** — Generate user-friendly explanation grounded in scheme records
7. **Version Update** — Detect policy changes, update records with new effective date

### Retrieval Comparison Experiments
| Method | Description |
|--------|-------------|
| BM25 | Keyword baseline (OpenSearch / rank_bm25) |
| Dense | Sentence-Transformer embeddings + FAISS |
| Hybrid | BM25 + Dense combined |
| Hybrid + Rerank | Hybrid + cross-encoder reranker |

Evaluation metrics: Precision@K, Recall@K, MRR, NDCG@K

### Eligibility Output Categories
- ✅ **Eligible** — All documented rules satisfied
- 🟡 **Possibly Eligible** — Most rules satisfied; one or more require verification
- ❌ **Not Eligible** — One or more hard rules not met
- 🔍 **Needs Verification** — Insufficient information to determine

---

## Open Tasks
- [ ] Implement PuLP MILP for capital structure optimization
- [ ] Build scheme JSONB schema in PostgreSQL
- [ ] Integrate rank_bm25 + Sentence-Transformers retrieval pipeline
- [ ] Add cross-encoder reranker for scheme ranking
- [ ] Connect scheme subsidy output to Module 3 funding gap calculation
- [ ] Expose Module 3 financial engine as a FastAPI deterministic service
