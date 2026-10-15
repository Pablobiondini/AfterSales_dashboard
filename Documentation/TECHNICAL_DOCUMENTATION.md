# Technical Documentation

## Model

Fact Table:
- SAP_cerrados

Supporting Tables:
- DimFecha
- DimFeriados

---

## Data Sources

### SharePoint

Library:
- Documentos Compartilhados

Path:

REPO_PBI_BD
→ RTAT
→ Cerrados

---

## SAP Export Compatibility

Supported sheet names:

- Data
- Sheet1

Fallback logic implemented in Power Query.

---

## Power Query

### Key Transformations

- Header normalization
- Dynamic sheet detection
- Column type enforcement
- Falla split into:
  - Falla 1
  - Falla 2
  - Falla 3

---

## Calculated Columns

### Season

Season starts:
- 1 April

Season ends:
- 31 March

Example:

Season 2025-2026

---

### Service Type

ZM → Carry In

Others → In Home

---

### Con Repuesto

Text empty or M/m:
- 0

Any other value:
- 1

---

### En Radio

Blank KM:
- Considered inside coverage

KM <= 50:
- SI

KM > 50:
- NO

---

### Valid Technical Visit

Flag:

1 = Included
0 = Excluded

Excluded Falla 1 values:

- Anulación
- Anulado
- Anulado/cliente no lleva equipo
- Anulacion de reapertura
- Cliente ausente (sin visita)
- Datos incorrectos
- Error de carga / repetición de reclamo
- Aviso Duplicado

---

## Measures

### QTY Cerrados

Primary volume measure.

### Coverage %

Cases within coverage range divided by total closed cases.

### First Time Fix %

Cases closed within 1 day after visit divided by visited cases.

### Pareto Analysis

Dynamic ranking using ALLSELECTED().

### TAT Business Days

Based on business day index stored in DimFecha.

### TAT Calendar Days

Date Close - Date Start / Visit logic.