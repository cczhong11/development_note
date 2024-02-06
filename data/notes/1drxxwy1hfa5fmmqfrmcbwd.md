
SLICK is a dedicated SLO(service level objectives). With SLICK, we are able to centralize SLI and SLO definitions to easily find and understand another service’s reliability; provide service owners with insights using high-retention, full granularity data for key service metrics not found in other tools; and integrate SLOs with various other workflows at the company to ensure that SLOs become a part of day-to-day work.

- Define SLOs in a unified way for our services
- Have up to per-minute granularity metric data with up to two years of retention
- Have standard visualizations and insights for SLI/SLO metrics
- Send periodic reliability reports to internal groups, allowing teams to use them for reliability reviews

# how to onboard

![](/assets/images/2022-12-27-15-08-22.png)

# using

- dashboard

![](/assets/images/2022-12-27-15-08-39.png)

- report
- cli

# Arch

![](/assets/images/2022-12-27-15-09-04.png)

- SLICK Configs: A config file written using SLICK’s DSL, committed by the user to the SLICK config store.
- SLICK Syncer: A service that synchronizes changes made to SLICK configs into SLICK’s config metadata storage.
- SLICK UI: These are the generated SLICK dashboards for every service. The SLICK UI also provides the index mentioned previously.
- SLICK Service: A server that provides an API that is able to answer queries such as “How to compute the SLO for a specific visualization?”. The server allows us to abstract away all the details around data placement and sharding, and it enables the caller to easily find the data needed.
- SLICK Data Pipelines: Pipelines that periodically run in order to capture SLI data over the long term.

## Data ingestion

![](/assets/images/2022-12-27-15-09-50.png)
