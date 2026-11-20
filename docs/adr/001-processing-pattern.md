# ADR 001: Simplified Lambda for Nordic Home (no Kafka, no Dataflow)

## Status
Accepted

Date: 2026-11-20

## Context
Shipper: SHP02 Nordic Home, an ocean-only furniture chain with 4 lanes (NLRTM → USHOU, DEHAM → USNYC, KRPUS → USLGB, SGSIN → BEANR). Its store openings are planned around container arrivals.

Targets: freshness 1 hour, reliability 75% on time on every lane, budget $45 per month.

Evidence from `docs/metrics.md`: 3 of 4 lanes miss the reliability target (KRPUS → USLGB 45.7%, NLRTM → USHOU 63.2%, DEHAM → USNYC 63.3%). Average delays on those lanes are 42 to 55 hours, so the problem is carrier performance, not how fast our data arrives. Tracking events arrive continuously; carrier rate files arrive once a day.

## Options considered
1. **Lambda (reference platform):** a speed layer (Pub/Sub, Kafka, Dataflow, BigQuery live) plus a batch layer (Cloud Storage, Dataproc, BigQuery curated). Meets the freshness target, but an always-on Dataflow job and Kafka may use most of the $45 budget.
2. **Kappa:** one streaming pipeline for everything, including rate files. Fewer layers, but rate files only arrive daily, so streaming them adds cost with no freshness benefit.
3. **Simplified Lambda:** events go from Pub/Sub straight into BigQuery through a BigQuery subscription (no Kafka, no Dataflow). Rate files stay batch, with a Dataproc job that runs on a schedule.

## Decision
We chose **Option 3, simplified Lambda**, because a 1-hour freshness target does not need stream processing, delays are measured in tens of hours, and the budget is small. The two data sources behave differently (continuous events, daily files), so keeping a separate speed path and batch path is still right, but each can be as simple as possible.

## Consequences
- **Cost:** no always-on Dataflow job and no Kafka. Dataproc runs only for the daily rate-file job. Expected to fit within $45 per month; to be confirmed in the Google Cloud pricing calculator.
- **Freshness:** events reach BigQuery within seconds to minutes, well inside the 1-hour target.
- **Complexity:** fewer components to build and run. Cleaning and joins happen in BigQuery SQL instead of in a stream job.
- **What gets harder:** if Nordic Home later needs near-real-time alerts or in-flight processing, such as per-event enrichment or windowed late-shipment detection, we would have to add Dataflow back.