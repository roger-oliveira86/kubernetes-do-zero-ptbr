# Wave migration with go/rollback criteria

This lab demonstrates a safe strategy for making progressive changes to a Kubernetes application.

## Objective

Run a wave migration while observing:

* error rate;
* latency;
* availability;
* traffic behavior;
* criteria for advancing;
* criteria for stopping or rolling back.

## Scenario

The application starts on a stable version. A new version is introduced to a small slice of traffic. The next wave is only released once the indicators stay within the defined limits.

## Go criteria

Before advancing to the next wave:

* errors stay below the defined limit;
* latency shows no relevant degradation;
* there's no CPU or memory saturation;
* logs and events show no new failures;
* the rollback procedure has been validated.

## Rollback criteria

The change must be stopped when there is:

* a sustained increase in errors;
* significant latency degradation;
* readiness or liveness failure;
* resource saturation;
* functional impact identified by the user.

The goal isn't to eliminate all risk. It's to make risk observable, limit the impact, and keep the decision reversible.

## Question for the reader

Which indicators would your team use to decide between advancing, pausing or rolling back?
