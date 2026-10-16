# Valid Technical Visit Filter Review

## Conclusion

The report definition enforces `SAP_cerrados[Visita Técnica Válida] = 1`
through a categorical **report-level filter** in
`AfterSales_dashboard_v1.2.Report/definition/report.json`. Its selected value
is serialized as `1L`.

No page-level `Visita Técnica Válida` filter was found in the page
definitions, and no visual-level filter on this column was found. Therefore,
the technical KPIs below are filtered in the current report by the report
filter, not by their DAX definitions. None is unfiltered in this report.

This is report-context enforcement, not model-level enforcement. The measures
do not explicitly add the valid-visit predicate, so use outside this report
(for example, a separate report or a query that does not inherit this filter)
does not guarantee valid visits. The inspected measures also do not remove the
report filter with `ALL`/`REMOVEFILTERS` on `Visita Técnica Válida`.

## KPI results

| Technical KPI | Model measure(s) / field | Filtered by measure? | Filtered by report/page/visual? | Classification |
|---|---|---|---|---|
| TAT Laboral | `TAT Laboral` | No. It averages `SAP_cerrados[TAT Laboral]` without a valid-visit predicate. | **Yes — report-level** `SAP_cerrados[Visita Técnica Válida] = 1`. No page- or visual-level valid-visit filter found. | Filtered by report |
| TAT Corridos | `TAT Corridos` | No. It averages `SAP_cerrados[TAT Corridos]` without a valid-visit predicate. | **Yes — report-level** `SAP_cerrados[Visita Técnica Válida] = 1`. No page- or visual-level valid-visit filter found. | Filtered by report |
| First Time Fix | `FVR(T)`; visuals may also use `SAP_cerrados[1stVisitRes]` directly | No. `FVR(T)` sums `1stVisitRes` and divides by `[QTY Cerrados]`; neither expression adds the valid-visit predicate. | **Yes — report-level** `SAP_cerrados[Visita Técnica Válida] = 1`. This applies to visuals using either the measure or the indicator column. | Filtered by report |
| FVR | `FVR(T)`, `FVR(5)`, `FVR(10)`, `FVR(15)` | No. Threshold variants add TAT thresholds but no valid-visit predicate. | **Yes — report-level** `SAP_cerrados[Visita Técnica Válida] = 1`. FVR page visuals also have service-type and date/measure filters; none is a valid-visit filter. | Filtered by report |
| Coverage | `%Coverage Range` (report field alias: `Coverage Range`) | No. It filters `En Radio = "SI"` for its numerator, but does not explicitly filter valid visits. | **Yes — report-level** `SAP_cerrados[Visita Técnica Válida] = 1`. No page- or visual-level valid-visit filter found. | Filtered by report |

## Filter scope checked

- **Report:** `report.json` contains the categorical filter on
  `SAP_cerrados[Visita Técnica Válida]` with included value `1L`.
- **Pages:** no page-level filter on `Visita Técnica Válida` was found.
- **Visuals:** no visual-level filter on `Visita Técnica Válida` was found.
  Other visual filters include date, service type, NPS type, and measure
  filters; these do not set the validity flag.
- **DAX:** the measures listed above do not set the validity flag themselves,
  and their filter expressions do not clear the report-level validity filter.

## Interpretation and caveat

In the current report, the “Not filtered at all” outcome applies to **none**
of these KPIs: all inherit the report-level filter. The measures remain
non-self-contained, however, and should not be assumed to enforce valid visits
in other report contexts.

The report-level filter only enforces the `Visita Técnica Válida = 1` flag. It
does not, by itself, establish that the FVR denominator is the count of records
with a nonblank visit date. The current FVR denominator is `[QTY Cerrados]`
(closed notices under the report filter); verify separately whether this
matches the intended “valid technical visits” denominator in the business
definition.
