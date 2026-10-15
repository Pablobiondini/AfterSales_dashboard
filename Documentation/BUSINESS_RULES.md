# Business Rules

## Overview

This document contains all business definitions used in the AfterSales Dashboard.

The objective is to ensure consistency across reporting, DAX calculations, Power Query transformations and future developments.

---

# Data Source

Source System:

- SAP

Primary Dataset:

- SAP_cerrados

The dashboard analyzes closed service orders and warranty cases.

Source: SAP exports processed through Power Query.

---

# Closed Cases

Definition:

A closed case represents a completed service order present in the SAP_cerrados dataset.

KPI:

- Closed Cases
- QTY Cerrados

Used as the primary volume indicator.

---

# Valid Technical Visit

Purpose:

Exclude records that do not represent a real technical intervention.

Flag:

- 1 = Included
- 0 = Excluded

Excluded Falla 1 values:

- Anulación
- Anulado
- Anulado/cliente no lleva equipo
- Anulacion de reapertura
- Cliente ausente (sin visita)
- Datos incorrectos
- Error de carga / repetición de reclamo
- Aviso Duplicado

Technical KPIs must use only valid visits.

Examples:

Included:
- Compressor replacement
- Electronic board repair
- Gas recharge

Excluded:
- Duplicate claim
- Administrative cancellation
- Incorrect customer information

---

# Service Type

Purpose:

Classify service modality.

Rule:

ZM = Carry In

Any other value = In Home

Categories:

- Carry In
- In Home

Used in:

- Service Mix Analysis
- Service Center Performance

---

# Spare Parts Indicator

Purpose:

Identify cases requiring spare parts.

Rule:

If spare part field is blank:
- 0

If spare part field contains:
- M
- m

Then:
- 0

Any other value:
- 1

Categories:

- Without Spare Parts
- With Spare Parts

Used in:

- Spare Parts Usage %
- Repair Analysis

---

# Coverage Range

Purpose:

Determine if service was performed within normal coverage distance.

Rule:

Blank KM:
- Inside coverage

KM <= 50:
- SI

KM > 50:
- NO

Categories:

- En Radio
- Fuera de Radio

Used in:

- Coverage %
- Geographic Analysis

---

# First Time Fix

Purpose:

Measure repair effectiveness.

Definition:

Cases closed within one day after the technical visit divided by total visited cases.

Formula Concept:

First Time Fix % =
FTF Cases / Valid Technical Visits

Higher values indicate greater repair efficiency.

---

# Turnaround Time (TAT)

Purpose:

Measure service completion speed.

Two metrics are available.

## TAT Calendar Days

Definition:

Difference between service start/visit date and closure date.

Includes weekends and holidays.

---

## TAT Business Days

Definition:

Difference between service start/visit date and closure date using business day index.

Excludes:

- Weekends
- Holidays defined in DimFeriados

Used as the primary operational TAT indicator.

---

# Season

Purpose:

Enable seasonal comparisons.

Season Start:

1 April

Season End:

31 March

Examples:

01-Apr-2025 → Season 2025-2026

15-Jan-2026 → Season 2025-2026

Used in:

- Season Comparison
- Trend Analysis

---

# Geographic Segmentation

Buenos Aires Province is segmented into:

- CABA
- GBA Norte
- GBA Oeste
- GBA Sur
- Buenos Aires Interior

All remaining provinces keep their original classification.

Used in:

- Coverage Analysis
- Service Center Distribution
- Geographic Performance

---

# Pareto Analysis

Purpose:

Identify the service centers generating the largest share of service volume.

Method:

Dynamic ranking based on selected filters.

Used in:

- Service Center Performance
- Volume Concentration Analysis

---

# Development Principles

All new KPIs must:

1. Respect Valid Technical Visit logic.
2. Use existing dimensions whenever possible.
3. Be documented before deployment.
4. Be registered in CHANGELOG.md.
5. Follow DAX_STANDARDS.md.