# AfterSales Dashboard

Power BI solution for After Sales Service Operations and Warranty Analysis.

## Purpose

This dashboard provides operational visibility into technical service performance, repair efficiency, workflow productivity, and customer service coverage.

The solution is designed to support operational, tactical and management-level decision making.

---

## Main KPIs

- Closed Cases
- TAT (Calendar Days)
- TAT (Business Days)
- First Time Fix Rate
- Coverage Range %
- Spare Parts Usage %
- Carry In vs In Home
- Service Center Pareto Analysis
- Season-over-Season Comparison

---

## Data Source

Source System:
- SAP

Primary Dataset:
- SAP_cerrados

Refresh Mode:
- Manual

---

## Business Rules

### Valid Technical Visit

The following Falla 1 values are excluded from technical KPIs:

- Anulación
- Anulado
- Anulado/cliente no lleva equipo
- Anulacion de reapertura
- Cliente ausente (sin visita)
- Datos incorrectos
- Error de carga / repetición de reclamo
- Aviso Duplicado

---

## Geographic Segmentation

Buenos Aires Province is divided into:

- CABA
- GBA Norte
- GBA Oeste
- GBA Sur
- Buenos Aires (Interior)

All other provinces remain unchanged.

---

## Repository Structure

```text
AfterSales_dashboard/
│
├── Report/
├── SemanticModel/
├── Documentation/
├── README.md
├── CHANGELOG.md
└── TECHNICAL_DOCUMENTATION.md
