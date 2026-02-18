# Optimizing Retail Inventory: A Risk-Based Allocation Framework

**Author:** Zachary Tang  
**Subtitle:** Balancing Service Levels and Capital Efficiency in the Chips Category  

---

## Overview
This project addresses the operational challenge of determining **optimal inventory levels under uncertain demand**. The primary objective is to balance:  

- **Service reliability:** minimizing stockouts  
- **Capital efficiency:** avoiding excessive inventory  

Using historical weekly transaction data, a statistical demand model was developed, and **safety stock** was calibrated with a volatility-adjusted buffer. The approach allows decision-makers to select inventory levels consistent with their risk appetite while controlling working capital.  

---

## Business Context
Inventory decisions directly impact:  
- Customer satisfaction  
- Revenue stability  
- Working capital allocation  
- Operational risk exposure  

**Trade-off:**  
- Too little stock → risk of stockouts and lost sales  
- Too much stock → inefficient capital usage  

The project aims to answer:  

> *What level of safety buffer appropriately balances uncertainty and capital usage?*

---

## Data
The analysis uses historical transaction data for chips products, including:  
- `PROD_NAME` – Product name  
- `PROD_QTY` – Quantity sold  
- `TOT_SALES` – Total sales per transaction  
- `DATE` – Transaction date  
- `STORE_NBR`, `LYLTY_CARD_NBR` – Identifiers  

**Preprocessing steps:**  
- Corrected date formatting  
- Removed duplicates and extreme outliers  
- Filtered out non-chip products (salsa, dips)  
- Categorized products by **Flavor Theme** and **Bag Size**  

---

## Exploratory Data Analysis
Key insights from EDA:  

1. **Volume drives revenue:**  
   - High revenue products are those sold most frequently and in higher quantities.  
   - Pricing differences play a secondary role.  

2. **Flavour and bag size trends:**  
   - Most popular flavours: **Cheese & Dairy**, **Plain & Salted**, **Meat & Savory**, **Specialty/Other**  
   - Bag sizes: Small (<175g) dominate >80% of total sales.  

3. **Temporal patterns:**  
   - Daily and weekly sales largely stable across the year.  
   - Isolated deviations in April, May, and December.  
   - Price segmentation shows demand stability across low and high-price products.  

4. **Structural dependencies:**  
   - Certain flavours are only sold in specific bag sizes.  
   - Inventory allocation must consider **size × flavour** cells rather than treating dimensions independently.  

---

## Inventory Strategy & Modelling
1. **Baseline Demand:**  
   - Weekly demand is approximately stationary with no significant trend.  
   - Static inventory policy is appropriate given the absence of structural seasonality.  

2. **Proportional Allocation:**  
   - Inventory allocated to **size × flavour cells** proportional to historical mean shares.  

3. **Volatility Buffer:**  
   - Residual fluctuations introduce stockout risk.  
   - Safety stock formula:  I_s,f = s̄_s,f * Baseline Demand + k * σ_s,f

Where: 
- `I_s,f` = Weekly inventory for size × flavour cell  
- `s̄_s,f` = Mean share of demand for that cell  
- `σ_s,f` = Standard deviation of weekly demand for that cell  
- `k` = Volatility multiplier  

### Backtesting & Sensitivity Analysis
- Conducted across `k` values from 0 to 1.5  
- Target overall stockout rate = 5%  
- Optimal multiplier: `k ≈ 1`, balancing inventory efficiency and service level

---

## Findings
- Weekly demand is stable; isolated spikes are episodic rather than systematic.  
- Flavour and bag-size proportions are consistent; peaks and troughs affect all categories proportionally.  
- Inventory must consider **size × flavour dependencies** to mitigate mix risk.  
- Volatility-adjusted proportional allocation reduces stockouts to 5% while minimizing excess inventory.  
- Static inventory policy is simpler operationally and statistically defensible.  

---

## Governance & Risk Considerations
- **Recalibration:** Update mean and volatility estimates periodically.  
- **Monitoring:** Track realized stockouts versus targets to adjust \(k\) if necessary.  
- **Structural Breaks:** Detect demand shifts due to promotions, competition, or supply disruptions.  

---

## Assumptions & Limitations
- Independent weekly demand shocks  
- Stable variance and approximate normality  
- Symmetric cost of stockouts and excess inventory  
- One-year historical data; extreme events may be underestimated  
- No explicit economic cost optimization  

---

## Potential Extensions
- Monte Carlo simulation for tail risk and extreme scenarios  
- Cost-based optimization to determine \(k\) by minimizing total expected cost  
- Scenario stress testing for demand surges, collapses, or increased volatility  

---

## Conclusion
- Adopt a **static, volatility-adjusted inventory policy**.  
- Allocate stock at the **size × flavour level** due to structural dependencies.  
- Include a **volatility buffer** (k ≈ 1) to mitigate stockout risk.  

This framework balances **service reliability**, **capital efficiency**, and operational simplicity while providing a robust, data-driven inventory strategy.  
 
