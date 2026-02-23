# Dirtbikes-BI-lab
# DirtBikes Sales Analytics — Data Warehouse, OLAP Cube & ETL (Mondrian + Pentaho PDI)

A curated **BI/OLAP case study** that demonstrates the full flow from **data modeling → ETL → OLAP cube → MDX analysis/dashboard** using the “DirtBikes” sales domain.

> **Note on data & executability:**  
> This repository focuses on **modeling and pipeline logic**. The original course dataset / DB instance is not redistributed here, so the project is presented as a **read-only portfolio** (artefacts + outputs). The cube/ETL definitions can still be reviewed and understood end-to-end.

---

## What you can see at a glance

- **Dashboard output (PDF):** `docs/dashboard.pdf`
- **OLAP / cube model diagram:** `docs/screenshots/hypercube-schema.png`
- **Mondrian cube definitions (XML):** `mondrian/`
- **ETL pipelines (Pentaho PDI .ktr/.kjb):** `etl/`

![Hypercube model](docs/schema & Dashboard/hypercube-schema-drawio 2.png)

---

## Project overview (what I built)

### 1) OLAP cube on an OLTP view (Mondrian)
A Mondrian cube defined on an OLTP-style join (OrderHead ⨝ OrderPosition), including calculated time keys and business measures.

**Dimensions / hierarchies**
- **Time**: Year → Month → Day (derived from `ORDERDATE`)
- **Product**: Division → ProductCategory → Product
- **Customer**: Country → SalesOrg → City → Customer

**Measures**
- `RevenueUSD` (sum)
- `DiscountUSD` (sum)
- `CoGMUSD` / Cost of Goods (sum)
- `SalesQuantity` (sum)

Artefact: `mondrian/cube_oltp_view.xml`

---

### 2) Star schema OLAP cube + aggregate table (Mondrian)
A cube defined against a **Star Schema** (fact + dimensions) including an **aggregate table** for performance.

**Dimensions**
- `PRODDIM` (Division / Category / Product)
- `CUSTDIM` (Country / SalesOrg / City / Customer)
- `TIMEDIM` (Year / Month / Day)

**Measures**
- `NOOFSALESORDERS`
- `SALESQUANTITY`
- `COSTOFGOODSSOLD`
- `REVENUE`
- `DISCOUNT`

**Aggregate table (Mondrian AggName)**
- Uses a pre-aggregated fact variant (see cube XML definitions)

Artefact: `mondrian/cube_star_agg.xml`

---

### 3) ETL pipelines (Pentaho Data Integration / Kettle)
ETL jobs and transformations that load and update the warehouse, including delta logic and customer change processing.

**ETL building blocks (examples)**
- Initial loads (dimensions/facts)
- Delta load logic (facts/products/customers)
- Exchange rate loading (currency support)
- Aggregate table load (for OLAP performance)
- Customer changes from CSV (As-Is / As-Of style tracking)

Artefacts: `etl/jobs/*.kjb` and `etl/transformations/*.ktr`

---

### 4) MDX analysis + dashboard output
MDX queries and a dashboard-style output document.

Artefact: `docs/dashboard.pdf`  
(If you want: I can also add `mdx_examples.md` with selected MDX snippets + explanations.)

---

## Repository contents (recommended layout)

> If you’re building this repo from your course ZIP: copy/rename the artefacts into the structure below.
