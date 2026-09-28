# SIM Soil Data Model

**INSPIRE-based soil data model for SIM O3 with extensions for soil monitoring.**

Developed by Lorenzo Giunchi, Marco Cauli, Francesco Minutella, Andrea Abbiati, Silvano Pecora, Claudia Cagnarini

This repository contains the soil data model of **SIM O3**, a relational data model for storing and exchanging soil data. It is built on the INSPIRE Soil application schema and extended with the information needed for soil monitoring. It is implemented in PostgreSQL with PostGIS.

> **Status: work in progress.** The schema documentation is available; the SQL schema, license and citation will be added before the first release.

## Background

O3 starts from the **EJP SOIL GeoPackage** <<https://zenodo.org/records/18246825>>, an implementation of the **INSPIRE Soil** <<https://inspire-mif.github.io/uml-models/approved/html/index.htm?goto=2:3:17:9065>> data specification, and is aligned with the INSPIRE-based model proposed by the **European Environment Agency (EEA)** for soil data reporting under the NEC Directive.

These models describe where soil was observed and how soil properties were measured, but they do not record much of the context needed to interpret and compare monitoring results over time. O3 keeps the INSPIRE core unchanged and adds this context as new tables and attributes.

## What SIM O3 adds

| Area | INSPIRE Soil | EEA model | SIM O3 |
|---|---|---|---|
| Land cover, land use and soil management | Separate INSPIRE themes, not linked to soil sites | Site classification only (MAES, EUNIS, protection status) | Land use (HILUCS), land cover with mosaics and vegetation type, soil management attributes per site and date |
| Sampling and sampler | Plot type and depth ranges | Plot or sample size, sampling depth | Sampler, equipment, sampling procedure, sampling area size and shape, sub-samples and their locations |
| Sample handling and measurement uncertainty | No dedicated attributes | No dedicated attributes | Transport, storage and preparation of samples, laboratory, numeric uncertainty and threshold operators for censored values |
| Soil biodiversity | No data structure | "Biological" category of observable properties | Biological forms (e.g. soil microarthropods) <<https://zenodo.org/records/14070537>> recorded through a hierarchical code list, searchable by taxonomic class |

## Model overview

The schema contains 55 tables, grouped as follows.

- **INSPIRE Soil core**: `soil_site`, `soil_plot`, `soil_profile`, `profile_element`, `soil_body`, `soil_derived_object`, `soil_theme_coverage` and their relationships.
- **Observations**, following the O&M / SensorThings pattern: `datastream`, `observation`, `observable_property`, `process`, `sensor`, `unit_of_measure`.
- **Sampling and sample handling**: sampling attributes on `soil_plot` and `soil_profile`, `soil_sub_sample`, `equipments`, `preparation_process`.
- **Land and site context**: `existing_land_use_object`, `existing_land_use_value`, `land_cover_unit`, `land_cover_observation`, `land_cover_observation_mosaic`, `land_management`, `disturbance_soil_site`, `contaminated_site`, plus the EEA site attributes on `soil_site`.
- **Access control** for multiple organisations: `organisation`, `group`, `permission`, `inspire_organisation`, `inspire_permission`.

Geometries are stored in ETRS89-LAEA (EPSG:3035).

## Documentation

- **Schema documentation**, generated with SchemaSpy: <https://claudiacagn.github.io/SIM-SoilDataModel/>. It includes the diagrams, all tables and columns, and the relationships between them.

## Repository structure

```
├── README.md
├── sql/            Database schema (DDL) — coming soon
└── docs/           SchemaSpy documentation (published with GitHub Pages)
```

## Getting started

Requirements: PostgreSQL with the PostGIS extension, installed in a separate schema (not in `public`).

Installation instructions will be added together with the SQL schema.

## License

A license has not been chosen yet. Until one is added, all rights are reserved.

## Funding

This work was carried out within **SIM – Sistema Avanzato ed Integrato di Monitoraggio e Previsione** (Advanced and Integrated Monitoring and Forecasting System) <<https://sim.mase.gov.it/portalediaccesso>> of the Italian Ministry of Environment and Energy Security (MASE), under the National Recovery and Resilience Plan (PNRR), Mission 2, Component 4, Investment 1.1 "Realizzazione di un sistema avanzato ed integrato di monitoraggio e previsione", funded by the European Union – NextGenerationEU.

## Feedback

Questions, suggestions and error reports are welcome through [GitHub Issues](../../issues).
