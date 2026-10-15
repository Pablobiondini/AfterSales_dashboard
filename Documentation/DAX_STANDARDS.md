# DAX Standards

## Purpose

This document defines DAX development standards for the AfterSales Dashboard.

Objectives:

- Maintain consistency.
- Improve readability.
- Reduce technical debt.
- Facilitate AI-assisted development.
- Optimize performance.

All new measures must follow these standards.

---

# Naming Convention

## Measures

Use business-friendly names.

Good:

[QTY Cerrados]

[TAT Business Days]

[TAT Calendar Days]

[Coverage %]

[First Time Fix %]

[Spare Parts %]

Bad:

[m1]

[Measure]

[Test]

[KPI_01]

---

## Variables

Use descriptive names.

Good:

VAR TotalCases

VAR ValidVisits

VAR CurrentSeason

VAR SelectedSC

Bad:

VAR A

VAR TMP

VAR X

---

## Boolean Variables

Prefix with Is.

Examples:

VAR IsValidVisit

VAR IsCurrentSeason

VAR IsCoverageCase

---

# Measure Structure

All measures must follow the same layout.

Example:

QTY Cerrados =
VAR Result =
    COUNTROWS ( SAP_cerrados )

RETURN
    Result

Rules:

1. Variables first.
2. Single RETURN statement.
3. Result variable preferred.
4. One logical block per variable.

---

# Formatting

Use DAX Formatter conventions.

Example:

Coverage % =
VAR CoveredCases =
    CALCULATE (
        [QTY Cerrados],
        SAP_cerrados[En Radio] = "SI"
    )

VAR TotalCases =
    [QTY Cerrados]

RETURN
    DIVIDE (
        CoveredCases,
        TotalCases
    )

Rules:

- One argument per line.
- Indent nested expressions.
- Separate variables with blank lines.
- Never write one-line complex measures.

---

# DIVIDE Usage

Always use DIVIDE().

Good:

DIVIDE (
    Numerator,
    Denominator
)

Bad:

Numerator / Denominator

Reason:

Prevents divide-by-zero errors.

---

# CALCULATE Usage

Use CALCULATE only when changing filter context.

Good:

CALCULATE (
    [QTY Cerrados],
    SAP_cerrados[En Radio] = "SI"
)

Avoid unnecessary CALCULATE wrappers.

---

# Filter Context

Prefer explicit filters.

Good:

CALCULATE (
    [QTY Cerrados],
    SAP_cerrados[Valid Technical Visit] = 1
)

Avoid ambiguous context assumptions.

---

# Valid Technical Visit Rule

Technical KPIs must respect Valid Technical Visit logic.

Examples:

- First Time Fix
- TAT
- Repair KPIs
- Productivity KPIs

Do not include excluded visits unless explicitly requested.

---

# Aggregation Standards

Preferred order:

1. Existing measures
2. SUM
3. COUNTROWS
4. DISTINCTCOUNT

Avoid repeating business logic.

Good:

Coverage % =
DIVIDE (
    [Coverage Cases],
    [QTY Cerrados]
)

Bad:

Repeating full calculations inside every measure.

---

# ALL and ALLSELECTED

Use carefully.

ALL()

Only for global calculations.

Example:

Overall Total

ALLSELECTED()

Preferred for Pareto and ranking calculations.

Example:

Service Center Pareto

---

# Iterator Functions

Use only when required.

Examples:

SUMX

AVERAGEX

RANKX

Avoid iterators when a simple aggregation works.

Performance first.

---

# Ranking Measures

Use RANKX.

Preferred structure:

RANKX (
    ALLSELECTED ( Dimension ),
    [QTY Cerrados],
    ,
    DESC
)

Used for:

- Pareto
- Top Service Centers
- Geographic Ranking

---

# Time Intelligence

Use DimFecha as the primary date table.

Do not create custom date logic inside measures when DimFecha already provides the context.

Preferred:

DATESYTD()

DATEADD()

SAMEPERIODLASTYEAR()

When applicable.

---

# Season Logic

Season calculations must use the Season column already created in the model.

Do not recreate season determination inside measures.

---

# Performance Guidelines

Avoid:

FILTER (
    EntireFactTable,
    ...
)

When a direct filter can be used.

Prefer:

CALCULATE (
    [QTY Cerrados],
    SAP_cerrados[Column] = Value
)

---

# Reusability

Create base measures first.

Example:

[QTY Cerrados]

Then:

[Coverage Cases]

Then:

[Coverage %]

Avoid embedding business logic repeatedly.

---

# Documentation

Every new KPI must include:

Purpose

Business definition

Formula description

Dependencies

Example:

Measure:
[Coverage %]

Purpose:
Measure service coverage compliance.

Depends on:
[Coverage Cases]
[QTY Cerrados]

Formula:
Coverage Cases / Total Cases

---

# Change Management

Every modification must:

1. Update CHANGELOG.md
2. Preserve backward compatibility when possible
3. Document business impact
4. Explain KPI changes

---

# AI Development Rules

When an AI agent creates a measure:

1. Reuse existing measures first.
2. Follow naming conventions.
3. Use variables.
4. Use DIVIDE().
5. Respect Valid Technical Visit rules.
6. Minimize FILTER().
7. Explain business purpose.
8. Document dependencies.
9. Update CHANGELOG.md.

No exceptions.
