---
layout: page
title: "Learning Inertial Odometry: From Specialized to Generalized"
description: Invited talk at the IROS 2026 workshop "Beyond Exteroception - Interoceptive Perception for Resilient Robotics"
permalink: /talks/iros2026/
nav: false
---

**Venue:** IROS 2026 workshop, _Beyond Exteroception: Interoceptive Perception for Resilient Robotics_<br>
**Location:** Room 403, Pittsburgh, PA, USA<br>
**Time:** September 27, 2026, 4:00–4:30 PM

In this talk, I share our recent progress in pushing the limits of learning-based inertial odometry, from specialized models toward generalizable inertial odometry across embodiments and domains.

{% include figure.liquid loading="eager" path="assets/img/talks/iros2026.jpg" class="img-fluid rounded z-depth-1" alt="IROS 2026 talk poster" %}

{% comment %} Add the recording here once available:
{% include video.liquid path="https://www.youtube.com/embed/VIDEO_ID" class="img-fluid rounded z-depth-1" %}
{% endcomment %}

## Summary

_Your camera might be blocked, your LiDAR might fail, but the IMU will never fail you._ IMUs are cheap, light and found on every robot, and they sense motion without looking at the world. Learned inertial odometry, however, long worked only for pedestrians. The talk walks through four steps that take it from one specialized setting to drones, legged robots and humanoids.

**1. Sensor model: learn the noise, keep the integration.** IMU integration is deterministic physics; the hard part is the noise. [AirIMU](https://airimu.github.io) makes integration and covariance propagation differentiable, so ground-truth poses supervise a learned noise model and its uncertainty. It works from two-dollar MEMS chips up to navigation-grade units on a helicopter. The learned covariance is _metrics-aware_: predicted uncertainty tracks the actual error, which is what [MAC-I²](https://mac-i2.github.io/) relies on for robust visual-inertial initialization and calibration.

<div class="row mt-3">
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/talks/iros2026/02-airimu-differentiable-integration.jpg" class="img-fluid rounded z-depth-1" alt="AirIMU: differentiable integrator and covariance propagation" %}
  </div>
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/talks/iros2026/03-metrics-aware-covariance.jpg" class="img-fluid rounded z-depth-1" alt="Metrics-aware covariance: predicted uncertainty follows the error" %}
  </div>
</div>

**2. Feature representation: stay in the body frame.** Earlier methods rotate IMU data into the global frame, which entangles the robot's attitude with the external force nonlinearly. In the raw body frame the two are linearly coupled, and the gravity direction tells the network the attitude. [AirIO](https://air-io.github.io) removes that one step, adds an attitude encoder, and improves drone odometry by roughly 10×. The same insight carries over to wheeled and legged robots in Tartan IMU.

{% include figure.liquid path="assets/img/talks/iros2026/04-airio-body-frame.jpg" class="img-fluid rounded z-depth-1" alt="AirIO: the global coordinate frame used by existing methods is suboptimal" %}

**3. Data scaling: from human to humanoid.** Collecting hundreds of hours of humanoid data is expensive and risky, but hundreds of hours of human motion with body-worn IMUs already exist. A unified representation turns humanoid joint encoders plus the pelvis IMU into synthetic IMUs at human joints, so a model trained only on human data transfers nearly zero-shot to a Unitree G1.

{% include figure.liquid path="assets/img/talks/iros2026/05-human-to-humanoid.jpg" class="img-fluid rounded z-depth-1" alt="Unified representation for human and humanoid motion" %}

**4. Self-supervision: learn from raw IMU data alone.** A model trained on one drone still drifts on another, and no robot comes with hundreds of hours of labels. [SSIO](https://ssio-imu.github.io/) uses de-biased IMU integration over short windows as the teacher for a velocity network. Across six datasets (pedestrians, two drones, two quadrupeds and a humanoid) it comes close to the supervised AirIO. On the robot demos, 40 minutes to an hour of unlabeled data was enough: a Unitree G1 humanoid, a Go1 quadruped in a Pittsburgh park, and ANYmal on snow.

<div class="row mt-3">
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/talks/iros2026/06-ssio-method.jpg" class="img-fluid rounded z-depth-1" alt="SSIO: IMU integration teacher supervises a student velocity network" %}
  </div>
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/talks/iros2026/07-ssio-embodiments.jpg" class="img-fluid rounded z-depth-1" alt="SSIO across six embodiments compared with supervised AirIO" %}
  </div>
</div>

**Why can the student beat its teacher?** The network only ever sees plain integration as supervision, yet it ends up more accurate than the integration. The evidence says it does not learn integration itself: a model trained on one robot does not transfer to another. It takes a shortcut and learns the embodiment's motion patterns. Here that is exactly what you want.

<div class="row mt-3">
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/talks/iros2026/08-student-surpasses-teacher.jpg" class="img-fluid rounded z-depth-1" alt="How can the student surpass its teacher?" %}
  </div>
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/talks/iros2026/09-gpt-moment-for-odometry.jpg" class="img-fluid rounded z-depth-1" alt="How far are we from a GPT moment for odometry?" %}
  </div>
</div>

**Looking ahead.** Each step removed one constraint: better sensor models, cross-embodiment features, scaling with data, then self-supervision for any new robot. The open question is how far we are from a "GPT moment" for odometry. In the Q&A I argued that the main bottleneck for deeper IMU modeling is data: rate-table and temperature-controlled recordings that public datasets still lack.

[LinkedIn post](https://www.linkedin.com/feed/update/urn:li:activity:7509973871211532288/)
