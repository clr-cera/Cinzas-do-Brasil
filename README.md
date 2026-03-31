# Cinzas do Brasil

**Multidimensional analysis of wildfire data**

A comprehensive Data Warehouse project analyzing the correlation between wildfire occurrences and climate patterns across Brazil.

## Problem Context

In 2024, **30.8 million hectares** were burned in Brazil. This project seeks to understand wildfire patterns and their relationship with climate conditions through data analysis.

## Project Objectives

- Extract INPE (Instituto Nacional de Pesquisas Espaciais) data on wildfires and climate
- Create a Data Warehouse with historical data from two sources
- Generate analytical visualizations for strategic decision-making
- Identify correlations between climate patterns and wildfire occurrence
- Support evidence-based fire prevention and resource allocation

## Data Sources

### Queimadas (Wildfires) - INPE
- Satellite-detected fire hotspots
- Geographic coordinates (latitude/longitude), biome
- Radiative Power (FRP), days without rain, fire risk index
- **17.5+ million tuples** (since 2003)

### Climate Data - SISAM/INPE
- Meteorological measurements and air quality
- Temperature, humidity, precipitation, pollutants (PM2.5, CO, NO₂, O₃, SO₂)
- **158.7+ million tuples** (since 2000)
- Hourly granularity at municipality level

### Geographic Data - IBGE
- Federal unit (state) and municipality directories
- Administrative hierarchies and regional classifications

**Total Data Volume**: 15+ GB

## Architecture Overview

### Dimensional Model

**Queimadas (Star Schema)**
- **Grain**: One fire hotspot per geographic point and minute
- **Facts**: FRP (additive), days without rain (semi-additive), fire risk (non-additive)
- **Dimensions**: Date, Location (with biome), Time of day

**Clima (Star Schema)**
- **Grain**: Climate measurements aggregated per municipality and hour
- **Facts**: Temperature, humidity, precipitation, pollutants (all additive)
- **Dimensions**: Date, Location, Time of day

## Technology Stack

### Infrastructure
- **Cloud Platform**: Google Cloud Platform
- **IaC**: Terraform for automated provisioning

### Database
- **Primary**: PostgreSQL with Citus for columnar storage

### ETL Pipeline
- **Main**: PySpark on Google Dataproc for large-scale processing
- **Prototyping/Testing**: Polars for data validation and local development
- **Orchestration**: Terraform-configured Dataproc jobs

### Visualization & Analytics
- **Visualization**: Apache Superset and SQL

## Getting Started

### Local Development (Docker)

```bash
# Start services
docker-compose up

# Run ETL pipeline
python main.py
```

### Infrastructure Deployment

See `provision/infra/README.md` and `provision/dataproc/README.md` for Terraform-based provisioning on Google Cloud Platform.

### Running Tests

```bash
pytest tests/
```

### Database Operations

```bash
# View optimization queries
cat queries/management/

# Apply schema migrations
psql < etl/migrations/001_init_schema_columnar.sql
```

## Key Insights

- Strong correlation identified between humidity and radiative power across regions and quarters
- Biome-specific fire patterns revealed through dimensional analysis
- Geographic and temporal concentration of fire incidents enables targeted prevention
- Air quality degradation peaks during dry season afternoons in Norte region
- Regional variations in precipitation patterns impact fire risk assessment
