---
title: exponential histograms and compaction
topics:
    - metrics
    - histograms
    - exponential histograms
references: 
    - https://dyladan.me/histograms/2023/05/04/exponential-histograms/
    - groxx
---

# Exponential Histograms

When using a histogram to measure a metric (e.g request latency, workflow duration, etc.) it is important to consider the the downstream effects of your bucketing strategy. 
If you ever need to re-bucket your data you will have a breaking change to any dependencies on it:
    - alerts
    - dashboards
    - other people's alerts & dashboards with a hard dependency

Using exponential histograms allows downsampling of a metric on the aggregators side with loss in fidelity (lower granularity of the metric) but without loss of mathematical precision when calculating alerts (e.g p95, p99, average over the histogram will remain accurate). 

This is due to perfect subsetting - a less granular exponential histogram is always a perfect subset of a more granular histogram (when using a scale factor to generate your buckets). For example, a metric with buckets using a 2^(n/8) factor can be downscaled to 2^(n/4) without loss of accuracy for downstream users. 

This is much better than needing to manually craft your own buckets, unless you have great foresight into the exact nature of the buckets you need (and will always need). 

## Open Telemetry Defaults

Open Telemetry defaults to 160 buckets for an exponential histogram. This will result in a lot of storage capacity used for each histogram (its essentially 160 timeseries in a chronosphere-like storage system). Being able to downsample intelligently is critical to reducing metric storage costs, while giving the opportunity for crucial use-cases to keep high data fidelity.  

## Choosing a Scale Factor

Assuming 160 buckets:

- 2^(n/8): 1ms to ~16mins 
- 2^(n/4): 1s to ~3years

