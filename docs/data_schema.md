# Data Schema Documentation

## Entity Relationship Diagram
```
┌─────────────────┐
│   SUPPLIERS     │
│  (Master Data)  │
└────────┬────────┘
         │
         │ 1:N
         │
┌────────▼────────────┐     N:M      ┌──────────────────┐
│ SUPPLIER_TIMESERIES │◄─────────────►│   DEPENDENCIES   │
│   (Weekly Metrics)  │               │ (Network Graph)  │
└────────┬────────────┘               └──────────────────┘
         │
         │ 1:N
         │
┌────────▼────────────┐               ┌──────────────────┐
│ DISRUPTION_EVENTS   │               │ EXTERNAL_FACTORS │
│  (Ground Truth)     │               │  (Weekly Risks)  │
└─────────────────────┘               └──────────────────┘
```

## Table Schemas

### 1. suppliers
Master table for all suppliers in network.

| Column | Type | Description |
|--------|------|-------------|
| supplier_id | str | Unique identifier (SUP-XXXX) |
| supplier_name | str | Company name |
| country | str | Primary operating country |
| region | str | Geographic region |
| tier | int | Supply chain tier (1, 2, 3) |
| category | str | raw_materials, components, logistics |
| criticality_score | float | Business importance (0-1) |
| latitude | float | Geographic coordinate |
| longitude | float | Geographic coordinate |

### 2. supplier_dependencies
Network edges representing supply relationships.

| Column | Type | Description |
|--------|------|-------------|
| from_supplier_id | str | Upstream supplier |
| to_supplier_id | str | Downstream supplier |
| dependency_strength | float | Volume share (0-1) |
| lead_time_normal_days | int | Expected lead time |
| relationship_start_week | int | When relationship began |

### 3. supplier_timeseries
Weekly operational metrics for each supplier.

| Column | Type | Description |
|--------|------|-------------|
| supplier_id | str | Foreign key |
| week_id | int | Week number (0 to N) |
| avg_lead_time_days | float | Mean lead time this week |
| lead_time_std_days | float | Lead time variability |
| order_fulfillment_rate | float | % orders fulfilled (0-1) |
| on_time_delivery_rate | float | % delivered on time (0-1) |
| order_volume | int | Number of orders |
| inventory_days | float | Days of inventory buffer |

### 4. disruption_events
Ground truth labels for disruptions.

| Column | Type | Description |
|--------|------|-------------|
| disruption_id | str | Unique ID (DIS-XXXX) |
| supplier_id | str | Foreign key |
| start_week | int | Week disruption started |
| end_week | int | Week disruption ended |
| severity | str | minor, moderate, severe |
| disruption_type | str | logistics, production, natural_disaster, geopolitical |
| cause_description | str | Optional details |

### 5. external_factors
Weekly regional risk indicators.

| Column | Type | Description |
|--------|------|-------------|
| week_id | int | Week number |
| region | str | Geographic region |
| weather_severity | float | 0-1 scale |
| geopolitical_risk | float | 0-1 scale |
| port_congestion | float | 0-1 scale |
| fuel_price_volatility | float | Coefficient of variation |
| pandemic_restriction_level | int | 0-5 scale |

external_factors
- week_id
- region
- weather_severity
- geopolitical_risk
- port_congestion
