---
title: Creating Alerts with Dynamic Thresholds in Azure Monitor
description: Create Alerts with machine learning based Dynamic Thresholds
author: yanivlavi
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 11/29/2018
ms.author: yalavi
ms.reviewer: mbullwin
---

# Metric Alerts with Dynamic Thresholds in Azure Monitor (Public Preview)

Metric Alert with Dynamic Thresholds detection leverages advanced machine learning (ML) to learn metrics' historical behavior, identify patterns and anomalies that indicate possible service issues. It provides support of both a simple UI and operations at scale by allowing users to configure alert rules through the Azure Resource Manager API, in a fully automated manner.

Once an alert rule is created, it will fire only when the monitored metric doesn’t behave as expected, based on its tailored thresholds.

We would love to hear your feedback, keep it coming at azurealertsfeedback@microsoft.com.

## Why and when is using dynamic condition type recommended?

1. **Scalable Alerting** – Dynamic Thresholds alerts rules can create tailored thresholds for hundreds of metric series at a time. Yet providing the same ease of defining an alert rule on a single metric. Using either the UI or the Azure Resource Manager API results in fewer alert rules to manage. The scalable approach is especially useful when dealing with metric dimensions or when applying to multiple resources, like all subscription resources. Which translates to a significant time saving on management and creation of alerts rules. [Learn more about how to configure Metric Alerts with Dynamic Thresholds using templates](alerts-metric-create-templates.md).

1. **Smart Metric Pattern Recognition** – Using our unique ML technology, we’re able to automatically detect metric patterns and adapt to metric changes over time, which may often include seasonality (Hourly / Daily / Weekly). Adapting to the metrics’ behavior over time and alerting based on deviations from its pattern relieves the burden of knowing the “right” threshold for each metric. The ML algorithm used in Dynamic Thresholds is designed to prevent noisy (low precision) or wide (low recall) thresholds that don’t have an expected pattern.

1. **Intuitive Configuration** – Dynamic Thresholds allow setting up metric alerts using high-level concepts, alleviating the need to have extensive domain knowledge about the metric.

## How to configure alerts rules with Dynamic Thresholds?

Alerts with Dynamic Thresholds can be configured through Metric Alerts in Azure Monitor. [Learn more about how to configure Metric Alerts](alerts-metric.md).

## How are the thresholds calculated?

Dynamic Threshold continuously learns the data of the metric series and tries to model it using a set of algorithms and methods., and tries to model it using a set of algorithms and methods. It detects patterns in the data such as seasonality (Hourly / Daily / Weekly), and is able to handle noisy metrics (such as machine CPU or memory) as well as metrics with low dispersion (such as availability and error rate).

The thresholds are selected in such a way that a deviation from these thresholds indicates an anomaly in the metric behavior.

## What does 'Sensitivity' setting in Dynamic Thresholds mean?

Alert threshold sensitivity is a high-level concept that controls the amount of deviation from metric behavior required to trigger an alert.
This option doesn't require domain knowledge about the metric like static threshold. The options available are:

- High – The thresholds will be tight and close to the metric series pattern. Alert rule will be triggered on smallest deviation, resulting in more alerts.
- Medium – Less tight and more balanced thresholds, fewer alerts than with high sensitivity (default).
- Low – The thresholds will be loose with more distance from metric series pattern. Alert rule will only trigger on large deviation, resulting in less alerts.

## What are the 'Operator' setting options in Dynamic Thresholds?

Dynamic Thresholds alerts rule can create tailored thresholds based on metric behavior for both upper and lower bounds using the same alert rule.
You can choose the alert to be triggered on one of the following three conditions:

- Greater than the upper threshold or lower than the lower threshold (default)
- Greater than the upper threshold
- Lower than the lower threshold.

## What do the advanced settings in Dynamic Thresholds mean?

**Failing Periods** - Dynamic Thresholds also allows you to configure “Number violations to trigger the alert”, a minimum number of deviations required within a certain time window for the system to raise an alert (the default time window is four deviations in 20 minutes). The user can configure failing periods and choose what to be alerted on by changing the failing periods and time window. This ability reduces alert noise generated by transient spikes. For example:

To trigger an alert when the issue is continuous for 20 minutes, 4 consecutive times in a given period grouping of 5 minutes, use the following settings:

![Failing periods settings for continuous issue for 20 minutes, 4 consecutive times in a given period grouping of 5 minutes](media/alerts-dynamic-thresholds/0008.png)

To trigger an alert when there was a violation from a Dynamic Thresholds in 20 minutes out of the last 30 minutes with period of 5 minutes, use the following settings:

![Failing periods settings for issue for 20 minutes out of the last 30 minutes with period grouping of 5 minutes](media/alerts-dynamic-thresholds/0009.png)

**Ignore data before** - Users may also optionally define a start date from which the system should begin calculating the thresholds from. A typical use case may occur when a resource was a running in a testing mode and is now promoted to serve a production workload, and therefore the behavior of any metric during the testing phase should be disregarded.

## Will slow behavior change in the metric trigger an alert?

Probably not. Dynamic Thresholds are good for detecting significant deviations rather than slowly evolving issues.

## How much data is used to preview and then calculate thresholds?

The thresholds appearing in the chart, before an alert rule is created on the metric, are calculated based on the last 10 days of historical data, once an alert rule is created, the Dynamic Thresholds will acquire additional historical data that is available and will continuously learn based on new data to make the thresholds more accurate.
