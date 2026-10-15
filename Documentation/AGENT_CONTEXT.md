# AfterSales Dashboard

## Business Purpose

Dashboard for After Sales Service Operations.

Main KPIs:

- Closed Cases
- TAT Calendar Days
- TAT Business Days
- First Time Fix %
- Coverage %
- Spare Parts Usage %
- Service Center Pareto

## Dataset

Main Fact:
SAP_cerrados

Dimensions:
DimFecha
DimFeriados

## Business Rules

Valid Technical Visit excludes:

- Anulación
- Anulado
- Anulado/cliente no lleva equipo
- Anulacion de reapertura
- Cliente ausente (sin visita)
- Datos incorrectos
- Error de carga / repetición de reclamo
- Aviso Duplicado

## Service Type

ZM = Carry In
Others = In Home

## Coverage

KM <= 50 = SI
KM > 50 = NO

Blank KM = SI

## Development Rules

- Never modify measures without documenting.
- Update CHANGELOG.md.
- Preserve model relationships.
- Follow Power BI best practices.