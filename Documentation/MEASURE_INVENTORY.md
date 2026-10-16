# Semantic Model Measure Inventory

## Scope and method

This inventory covers all 20 DAX measures defined in
`AfterSales_dashboard_v1.2.SemanticModel/definition/tables/Medidas.tmdl`.
The calculated column `QTYconVisita` in that table is not a measure and is
therefore out of scope.

Report usage is based on measure field bindings in the report visual
definitions. Dependencies list direct DAX references; column references are
also direct. Usage through another measure is called out as indirect. Page
aliases can differ from the model measure name when a report field was renamed.

## Measures

| Measure name | DAX category | Business purpose | Referenced measures | Referenced columns | Report page usage | Optimization opportunities |
|---|---|---|---|---|---|---|
| `TAT Laboral` | Average / operational duration | Average business-day turnaround time for closed service records. | — | `SAP_cerrados[TAT Laboral]` | KPI Overall; KPI YoY; Service Type; FVR | Measure itself has no valid-visit filter; current report applies one globally. Reuse a valid-visit base measure or retain an explicit predicate for use outside this report. |
| `TAT Corridos` | Average / operational duration | Average calendar-day turnaround time. | — | `SAP_cerrados[TAT Corridos]` | KPI Overall; KPI YoY; By Area; By Provider (Group); By provider (Individual) | Measure itself has no valid-visit filter; current report applies one globally. Preserve this filter when the measure is reused elsewhere. |
| `QTY Cerrados` | Distinct-count base / volume | Counts distinct notices with a nonblank closure date; primary closed-case volume. | — | `SAP_cerrados[N° de Aviso]`; `SAP_cerrados[Fecha Cierre]` | KPI Overall; By Area; By Provider (Group); By provider (Individual) | Keep as the reusable closed-volume denominator; inspect distinct-count cost if notice cardinality or fact volume grows substantially. |
| `%5D` | Threshold attainment ratio | Share of closed notices with business-day TAT no greater than 5. | `[QTY Cerrados]` | `SAP_cerrados[N° de Aviso]`; `SAP_cerrados[Fecha Cierre]`; `SAP_cerrados[TAT Laboral]` | KPI Overall; KPI YoY; Service Type; By Area; By Provider (Group); By provider (Individual); FVR | Reuse `[QTY Cerrados]` for the filtered numerator; current report applies valid-visit filtering globally, but the measure itself does not. Set an explicit percentage format. |
| `%10D` | Threshold attainment ratio | Share of closed notices with business-day TAT no greater than 10. | `[QTY Cerrados]` | `SAP_cerrados[N° de Aviso]`; `SAP_cerrados[Fecha Cierre]`; `SAP_cerrados[TAT Laboral]` | KPI Overall; KPI YoY; By Area; By Provider (Group); By provider (Individual) | Same opportunity as `%5D`: reuse the base count and do not rely on the current report-level filter when reused elsewhere. Set an explicit percentage format. |
| `% Con Repuesto` | Average / indicator proportion | Estimates the share of records marked as requiring spare parts by averaging the 0/1 indicator. | — | `SAP_cerrados[Con Repuesto]` | KPI Overall; KPI YoY; By Provider (Group) | Confirm whether the indicator should be restricted to valid technical visits; use a base count ratio if the business definition is based on cases rather than an unweighted row average. |
| `%Coverage Range` | Coverage ratio | Share of closed notices marked “SI” for in-range service coverage. | `[QTY Cerrados]` | `SAP_cerrados[N° de Aviso]`; `SAP_cerrados[En Radio]` | KPI Overall (bound field; report query alias is `Coverage Range`) | Current report applies valid-visit filtering globally; the measure itself does not. Use `[QTY Cerrados]` as the numerator base and denominator, and add an explicit percentage format. |
| `%TATcorr+30` | Average / threshold proportion | Share of records with calendar-day TAT of at least 30, via the 0/1 `TATcorr≥30` column. | — | `SAP_cerrados[TATcorr≥30]` | KPI Overall; KPI YoY | Apply the valid-visit population if this is intended as a technical TAT KPI. Confirm expected percentage formatting. |
| `FVR(T)` | First-time-fix ratio | Share of closed notices flagged as resolved within one day of visit, using the precomputed `1stVisitRes` indicator. | `[QTY Cerrados]` | `SAP_cerrados[1stVisitRes]` | KPI Overall; FVR | Current report applies valid-visit filtering globally; the measure itself does not. Verify the denominator is visited cases, per the business definition. |
| `Rank Taller` | Ranking | Dense descending rank of service centers by closed-notice volume in the selected context. | `[QTY Cerrados]` | `SAP_cerrados[Centro de Servicio]` | Indirect: By Provider (Group), through `Reparaciones Acumuladas` → `Pareto %` → `Pareto Flag` | Ranking is appropriate for this use. If the number of service centers grows, inspect the cost of reevaluating the rank within the cumulative calculation. |
| `Reparaciones Acumuladas` | Cumulative / Pareto helper | Closed-notice volume accumulated through the current service-center rank. | `[Rank Taller]`; `[QTY Cerrados]` | `SAP_cerrados[Centro de Servicio]` | Indirect: By Provider (Group), through `Pareto %` → `Pareto Flag` | The `FILTER(ALLSELECTED(...))` iterator evaluates `[Rank Taller]` for each selected center; profile with realistic center counts and consider a simpler cumulative pattern if it is a bottleneck. |
| `Pareto %` | Cumulative proportion / Pareto helper | Cumulative closed-notice share for the ranked service centers. | `[Reparaciones Acumuladas]`; `[QTY Cerrados]` | `SAP_cerrados[Centro de Servicio]` | Indirect: By Provider (Group), through `Pareto Flag` | Shares the nested ranking cost of the cumulative helper; avoid recalculating the chain redundantly in additional visuals. |
| `Pareto Flag` | Classification / threshold label | Labels service centers as “Top 80%” or “Resto 20%” based on cumulative share. | `[Pareto %]` | — | By Provider (Group) | No obvious standalone optimization; keep the label dependent on the shared Pareto calculation. |
| `NPS CC` | NPS score | Computes Call Center NPS as (promoters − detractors) / Call Center responses × 100. | — | `NPS_Base[NPS_Clase]`; `NPS_Base[Tipo_NPS]` | Customer Care | The three `COUNTROWS` aggregations are straightforward. A shared response-count/base pattern could reduce duplicated filter logic if more NPS measures are added. |
| `NPS ST` | NPS score | Computes Servicio Técnico NPS as (promoters − detractors) / Servicio Técnico responses × 100. | — | `NPS_Base[NPS_Clase]`; `NPS_Base[Tipo_NPS]` | Customer Care | Same opportunity as `NPS CC`; preserve the separate response-type filter. |
| `FVR(5)` | Thresholded first-time-fix ratio | Share of all closed notices with `1stVisitRes` whose business-day TAT is no greater than 5. | `[QTY Cerrados]` | `SAP_cerrados[1stVisitRes]`; `SAP_cerrados[TAT Laboral]` | FVR | Current report applies valid-visit filtering globally; the measure itself does not. Verify visited-case denominator logic; reuse the existing base measure consistently. |
| `FVR(10)` | Thresholded first-time-fix ratio | Share of all closed notices with `1stVisitRes` whose business-day TAT is no greater than 10. | `[QTY Cerrados]` | `SAP_cerrados[1stVisitRes]`; `SAP_cerrados[TAT Laboral]` | KPI Overall; FVR | Same population/denominator issue as `FVR(5)`; current report-level filtering is not inherent to the measure. |
| `FVR(15)` | Thresholded first-time-fix ratio | Share of all closed notices with `1stVisitRes` whose business-day TAT is no greater than 15. | `[QTY Cerrados]` | `SAP_cerrados[1stVisitRes]`; `SAP_cerrados[TAT Laboral]` | FVR | Same population/denominator issue as `FVR(5)`; current report-level filtering is not inherent to the measure. |
| `%RRR-60` | Repeat-repair ratio | Share of closed notices flagged for a prior repair within 60 days. | `[QTY Cerrados]` | `SAP_cerrados[RRR-60 Flag]` | KPI Overall (bound field; report query alias is `RRR-60 %`) | Measure-level logic reuses `[QTY Cerrados]`. Review the expensive calculated-column flag logic described under performance risks and confirm valid-visit eligibility. |
| `%RRR-30` | Repeat-repair ratio | Share of closed notices flagged for a prior repair within 30 days. | `[QTY Cerrados]` | `SAP_cerrados[RRR-30 Flag]` | No report page binding found | Measure-level logic reuses `[QTY Cerrados]`. Review the expensive calculated-column flag logic described under performance risks and confirm valid-visit eligibility. |

## Findings

### Duplicate measures

No exact duplicate measure expressions were found. Several measures are
parallel variants rather than duplicates:

- `%5D` and `%10D` use different TAT thresholds.
- `FVR(5)`, `FVR(10)`, and `FVR(15)` use different thresholds; `FVR(T)` is the
  unthresholded counterpart.
- `NPS CC` and `NPS ST` calculate the same NPS formula for different response
  types.
- `%RRR-30` and `%RRR-60` use different windows.

### Unused measures

- `%RRR-30` has no report visual binding and is not referenced by another
  measure. It is unused in the checked report definition.
- All other measures are bound by a report visual directly or are part of the
  Pareto dependency chain used by **By Provider (Group)**.

The report contains two fields with legacy query aliases, but their field
properties bind to the current model names: `%Coverage Range` appears as
`Coverage Range`, and `%RRR-60` appears as `RRR-60 %`. These are counted as
report usage, not as unused measures.

### DAX_STANDARDS.md deviations

- **Valid Technical Visit rule:** The relevant measures do not implement the
  validity predicate themselves, but `report.json` currently enforces
  `SAP_cerrados[Visita Técnica Válida] = 1` as a report-level filter. Thus the
  listed KPIs are filtered in this report, but the measures are not
  self-contained when reused outside it. See
  `VALID_TECHNICAL_VISIT_FILTER_REVIEW.md`. FVR denominators still use
  `[QTY Cerrados]`; confirm they match the business definition's visited-case
  denominator.
- **Measure structure:** Most measures omit the documented `VAR` / single
  `RETURN` / preferred `Result` layout. The existing NPS measures use a single
  `RETURN` but do not assign a final `Result`; `Reparaciones Acumuladas` uses
  a variable and single `RETURN` but does not use `Result`. This is a
  consistency/readability deviation, especially for future edits.
- **Naming:** `%5D`, `%10D`, `%TATcorr+30`, `FVR(T)`, and the numbered FVR
  variants are compact/abbreviated rather than self-explanatory business names
  as specified by the naming convention.
- **Reusable base logic:** `%5D`, `%10D`, and `%Coverage Range` repeat the
  closed-notice distinct-count logic instead of using `[QTY Cerrados]` for the
  numerator. The standards prefer existing measures and avoiding repeated
  business logic.
- **Formatting:** `%5D`, `%10D`, and `%Coverage Range` return ratios but lack
  an explicit percentage format in the measure metadata. `%TATcorr+30` also
  averages a 0/1 indicator while carrying a general integer format. Report
  visuals may override formats; verify the actual rendered output.

The plain aggregation measures (`AVERAGE`, `SUM`, or simple ratios) do use
`DIVIDE` where a division is performed. No measure-level division operator
violation or `FILTER` over the entire fact table was found.

### Performance risks

1. **High — repeat-repair calculated columns:** `SAP_cerrados[RRR-60 Flag]`
   and `[RRR-30 Flag]` each scan `FILTER(ALL(SAP_cerrados), ...)` for every
   fact row to find earlier same-serial records. This can approach quadratic
   refresh work as the fact table grows. The two columns duplicate most of
   their scan and comparison logic, differing mainly in the 60/30-day window.
   Consider computing repeat flags upstream or using a more scalable
   pre-aggregated/indexed approach.
2. **Medium — nested Pareto rank evaluation:** `Reparaciones Acumuladas`
   iterates selected service centers and evaluates `[Rank Taller]` inside the
   filter; that rank itself uses `RANKX`. This can become expensive for
   high-cardinality service-center lists or many concurrent visuals.
3. **Low/conditional — repeated fact distinct counts:** `%5D` and `%10D`
   independently perform filtered `DISTINCTCOUNT` operations. The base-count
   pattern can improve consistency and reduce duplicated expression work,
   although the engine may reuse some work within a query.
4. **Low/conditional — NPS count repetition:** Each NPS measure scans its
   filtered response set three times with `COUNTROWS`. This is likely modest
   for a survey table but becomes more material as additional NPS variants
   are introduced.

The TAT averages and `LOOKUPVALUE` calculations are over calculated columns;
they are refresh-time work rather than repeated measure-time iterators.
