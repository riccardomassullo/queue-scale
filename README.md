# queue-scale

**Autoscale EC2 fleets on real PHP-FPM saturation — not fake CPU signals.**

---

## The uncomfortable truth

If you're scaling your PHP stack on:

* CPU %
* request count
* generic host metrics

…you are probably scaling **too late** or **for the wrong reason**.

In real production systems:

* CPU can look fine while users are already waiting
* Apache can appear “busy” without real backend pressure
* ALB traffic doesn’t tell you if PHP is saturated
* Keep-alive, TLS, and slow clients distort everything

👉 The result:

* random latency spikes
* scaling that reacts too late
* wasted compute
* debugging at 3AM

---

## What actually matters

When your system is under real pressure:

👉 **requests start queueing inside PHP-FPM**

That’s the signal that matters.

---

## What queue-scale does

`queue-scale` gives you autoscaling based on **real saturation signals**:

* `PhpFpmListenQueue` → users are waiting
* `PhpFpmMaxChildrenReached` → hard saturation
* `ApacheBusyRatio` → secondary diagnostic signal

Instead of guessing, you scale on what actually affects users.

---

## What you get (public repo)

This repo is fully usable and includes:

* ✔️ lightweight collector (bash, no dependencies)
* ✔️ systemd service + timer
* ✔️ basic Terraform autoscaling module
* ✔️ architecture and install docs
* ✔️ minimal working setup

You can deploy this today and improve your scaling.

---

## Why this exists

Because this happens all the time:

```
CPU: 35%
Users: timing out
Autoscaling: doing nothing
```

And nobody explains why.

---

## Mental model shift

Bad scaling:

```
CPU high → scale
```

Better scaling:

```
Queue exists → users already waiting → scale
```

Best scaling:

```
Queue rising + max children reached + latency worsening → scale aggressively
```

That’s what `queue-scale` is built for.

---

## Who this is for

You’ll care about this if:

* you run PHP-FPM in production
* you use EC2 + ASG + ALB
* you’ve seen unexplained latency spikes
* CPU-based scaling has failed you
* you want **predictable scaling behavior**

---

## Quick start

1. Enable:

   * PHP-FPM status endpoint
   * Apache `/server-status?auto`

2. Install collector:

```bash
./scripts/install.sh
```

3. Attach IAM role:

* `cloudwatch:PutMetricData`

4. Apply Terraform example

5. Watch scaling react to **real queue pressure**

---

## Example

### Without queue-scale

* CPU: 40%
* latency: 1.4s
* autoscaling: idle

### With queue-scale

* queue: 8
* scale-out triggered immediately
* latency drops to ~400ms

---

## Sponsorware model

This project is funded via **Sponsorware**:

👉 Pay for **early access + production-grade version**
👉 Wait → get a delayed public version

---

## Public vs Sponsor edition

### Public repo

* core collector
* basic Terraform module
* delayed releases
* minimal examples

### Sponsor edition (queue-scale-insiders)

* advanced scaling module
* dashboards (CloudWatch / Grafana)
* tuning guide (real thresholds)
* panic-mode scaling logic
* troubleshooting playbooks
* production scenarios
* multi-pool support

---

## Pricing (intentionally cheap)

* **€5/month** → Early access
* **€15/month** → Production pack
* **€35/month** → Team / Pro
* **€99 one-time** → Lifetime

👉 If this saves you even **1 hour of debugging**, it already paid for itself.

---

## “Can’t I build this myself?”

Yes.

The question is:

👉 Do you want to spend days discovering:

* which signals lie
* which thresholds actually work
* how not to create flapping scaling
* how to combine queue + latency safely

Or spend €5 and skip that?

---

## What people are actually paying for

Not code.

They’re paying for:

* correct signals
* sane defaults
* production-safe behavior
* fewer surprises under load

---

## Roadmap

* [x] basic collector
* [x] basic Terraform module
* [ ] advanced scale-in
* [ ] panic-mode scaling
* [ ] dashboards
* [ ] multi-pool support
* [ ] nginx support

---

## Sponsor access

If you want:

* latest releases immediately
* battle-tested thresholds
* dashboards
* tuning + troubleshooting
* real-world scenarios

👉 become a sponsor

---

## Final thought

If your autoscaling logic has ever surprised you…

…it’s probably wrong.

---

## License

See LICENSE.
