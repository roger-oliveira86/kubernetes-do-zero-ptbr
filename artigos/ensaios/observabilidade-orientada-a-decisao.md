---
title: "Observability isn't a dashboard: it's decision support"
status: publicado-no-repositorio
tema: "How metrics, logs and traces help assess change, impact and the next safe step"
---

# Observability isn't a dashboard: it's decision support

> Sanitized essay. Contains no identifiable data, systems or incidents.

## Thesis

A dashboard can be green, and a change can still not be safe.

Monitoring answers whether a threshold was crossed. Observability needs to help answer a harder question: **given this behavior, what's the lowest-risk next step?**

That difference changes telemetry's role. It stops being just a collection of graphs and starts backing engineering decisions.

## Context

Changes in production are inevitable: a new release, a configuration change, a dependency that started responding differently, or a shift in load.

The risk isn't only in detecting that something went wrong. It's in failing to quickly tell apart:

- a deviation with no relevant impact;
- a problem localized to one journey;
- a degradation that requires stopping the change;
- and an incident that calls for an immediate rollback.

Without that context, the team tends to decide by gut feel, time pressure, or whichever graph looks the most alarming.

## Decision or trade-off

A mature operational investigation starts with four questions:

1. **What changed?**
   Version, configuration, dependency, capacity or traffic need to be visible in the same context as the telemetry.

2. **Who was affected?**
   Infrastructure metrics are necessary, but they don't replace signals from the user journey: success, latency, availability and business behavior.

3. **What's the scope?**
   Logs show specific events; traces connect distributed calls; metrics show trend and proportion. None of these signals is enough on its own.

4. **What's the next safe action?**
   Advancing, pausing, reducing scope, applying mitigation, or running a rollback are different decisions. The evidence needs to make these paths comparable.

The trade-off isn't "more dashboards versus fewer dashboards." It's investing in signals that reduce uncertainty before a change grows its *blast radius*.

## Evidence

In a distributed architecture, the three signals complement each other:

- **Metrics** show that a deviation exists and its size;
- **Traces** show where the experience degraded as it crossed components;
- **Logs** help explain the concrete event, as long as they carry context and correlation.

The usefulness shows up when they connect to an explicit decision.

For example: a latency increase after a change doesn't automatically call for a rollback. First you need to compare the affected journey, the success rate, the propagation to dependencies and the error-budget consumption. If the impact is growing, reaches users and threatens the reliability target, stopping the change stops being a subjective reaction and becomes a defensible decision.

This is also the basis for responsible AIOps: a model can help correlate signals and suggest hypotheses, but it doesn't replace the policy that defines limits, owners, stop criteria and a safe way out.

## Next safe step

Before building a new dashboard, pick a recurring change, or one with known risk, and answer:

- which signal shows real user impact;
- which change needs to appear on the timeline;
- which combination of evidence stops the rollout;
- and who decides between proceeding, pausing or rolling back.

If these answers don't exist, the problem isn't a missing panel. It's the lack of an operational contract for deciding under uncertainty.

---

## References

- [Google SRE Workbook — Monitoring Systems with Advanced Analytics](https://sre.google/workbook/monitoring/)
- [Google SRE Workbook — Implementing SLOs](https://sre.google/workbook/implementing-slos/)
- [OpenTelemetry — Unified observability signals](https://opentelemetry.io/)
