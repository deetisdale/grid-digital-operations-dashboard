# Grid & Digital Operations — Executive Dashboard

A blended executive dashboard for a utility company, combining grid
reliability, financial performance, and safety KPIs with IT system
health and engineering delivery metrics into a single governed view.

Built twice, in two different BI tools, to demonstrate the same data
modeling and analytical thinking across platforms:

- **[`/powerbi`](./powerbi)** — Full Power BI implementation: star-schema
  data model, 20+ DAX measures (including SAIDI/SAIFI reliability
  indices and budget variance analysis), custom theme, and exact
  layout spec. This version was actually built end-to-end in Power BI
  Desktop — see [`powerbi/case-study.md`](./powerbi/case-study.md) for
  the full writeup.
- **[`/lookml`](./lookml)** — Looker/LookML implementation: the same
  data model expressed as LookML views, explores, and a dashboard
  definition. (Looker requires a live warehouse connection to actually
  run — this folder is the code artifact; see
  [`lookml/README.md`](./lookml/README.md) for what it would take to
  deploy it live.)

All data in both versions is synthetic — no real utility company data
is used.

## Why two implementations

Data modeling and DAX/LookML both express the same underlying
relational thinking (star schema, dimension tables, measures vs.
dimensions) in different syntax. Building the same dashboard twice
was a deliberate way to demonstrate that the skill is the modeling
approach, not memorized syntax in one specific tool.

## Skills demonstrated

- Star-schema data modeling and relationship design
- DAX and LookML measure development, including multi-step derived
  calculations (SAIDI/SAIFI)
- Budget variance analysis
- Utility industry domain knowledge (SAIDI/SAIFI are real, industry
  standard reliability indices, not generic placeholder KPIs)
- Audience-aware dashboard design (controller-first layout hierarchy)
- Cross-platform BI literacy (Power BI and Looker)
