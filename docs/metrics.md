# Metrics: SHP02 · Nordic Home

On time = arrived no more than 24 hours after planned arrival.
Reliability target: **75% on time on every lane**.

| Lane | Trade lane | Shipments | Avg delay (h) | On-time % | Target | Meets target? |
|---|---|---|---|---|---|---|
| KRPUS → USLGB | Transpacific | 35 | 55.5 | 45.7 | 75% | ❌ No |
| NLRTM → USHOU | Transatlantic | 38 | 42.3 | 63.2 | 75% | ❌ No |
| DEHAM → USNYC | Transatlantic | 30 | 53.8 | 63.3 | 75% | ❌ No |
| SGSIN → BEANR | Asia-North Europe | 28 | 6.6 | 100.0 | 75% | ✅ Yes |

## Which carriers drive the misses?

Carriers with the most shipments on each failing lane:

| Lane | Carrier | Shipments | On-time % |
|---|---|---|---|
| KRPUS → USLGB | HRZ | 21 | 61.9 |
| KRPUS → USLGB | NWL | 9 | 0.0 |
| NLRTM → USHOU | HRZ | 22 | 77.3 |
| NLRTM → USHOU | NWL | 6 | 66.7 |
| DEHAM → USNYC | HRZ | 16 | 50.0 |
| DEHAM → USNYC | KST | 6 | 50.0 |

## Findings
- **KRPUS → USLGB is the worst lane (45.7%).** NWL has 0% on time over 9 shipments (avg delay 74.7 h), and HRZ, which carries most of the volume, reaches only 61.9%.
- **DEHAM → USNYC:** HRZ carries 16 of 30 shipments at only 50% on time.
- **NLRTM → USHOU:** HRZ meets the target (77.3%), but small carriers ATL and ORS have 0% on time (very few shipments, so treat these with caution).
- **SGSIN → BEANR meets the target at 100%** with every carrier.
- **NWL is inconsistent across lanes:** 100% on DEHAM → USNYC and SGSIN → BEANR, but 0% on KRPUS → USLGB.
- **Caveat:** many carrier/lane combinations have fewer than 5 shipments, so those percentages are weak evidence.