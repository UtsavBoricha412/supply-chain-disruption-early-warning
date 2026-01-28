# Supply Chain Disruption Early Warning System

## Problem Statement
Given a multi-tier supply chain network with weekly operational metrics and external risk factors, predict which suppliers face elevated disruption risk in the next 30 days, and issue persistent early warnings with explanations when risk exceeds acceptable thresholds.

## Definitions

### Disruption
A supplier is considered **disrupted** when either:
- Average lead time increases by **more than 50%** compared to a rolling **8-week historical baseline**, OR
- Order fulfillment rate drops by **more than 20%** compared to a rolling **8-week historical baseline**
- AND the condition persists for **at least 2 consecutive weeks**

The rolling 8-week baseline is used to balance responsiveness to recent changes with robustness against short-term noise.

### Early Warning
An **early warning alert** is issued when:
- Composite risk score > **0.7** for **≥2 consecutive weeks**, AND
- Predicted probability of disruption within the next **30 days** exceeds **0.65**

The risk score represents a normalized ensemble output combining survival risk, time-series instability, and network dependency exposure.

## Scope

### In Scope
- Supplier-level disruptions (not product- or shipment-level)
- 30-day prediction horizon
- Weekly time granularity
- Multi-tier supplier dependency networks
- External risk factors (weather, geopolitical, logistics conditions)

### Out of Scope
- Real-time or sub-daily predictions
- Individual shipment tracking
- Financial or pricing disruptions
- Demand-side forecasting

## Success Metrics

### Primary Metrics
- **Precision@20**  
  Percentage of actual disruptions among the top 20 highest-risk suppliers each week  
  **Target:** >65%

- **Early Detection Rate**  
  Percentage of disruptions warned at least **2 weeks before occurrence**  
  **Target:** >60%

### Secondary Metrics
- **False Alert Rate**  
  Percentage of alerts that do not result in a disruption  
  **Target:** <15%

- **Average Lead Time**  
  Mean time between first alert and actual disruption  
  **Target:** ≥2 weeks

- **C-index (Concordance Index)**  
  Measures ranking quality of time-to-disruption predictions in survival models  
  **Target:** >0.70

## Key Assumptions
1. Supplier disruptions exhibit early operational signals detectable from historical data
2. Disruptions propagate probabilistically through supplier dependency networks
3. External risk factors materially influence disruption likelihood
4. Weekly granularity provides sufficient resolution for actionable decision-making
