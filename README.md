# 🎫 Improving Generative Robot Policies With A Single Noise Vector

  <div align="center">
    <h2>
      <a href="https://golden-tickets.github.io/">🔗 Project website</a>
    </h2>
  </div>

Videos comparing the base policy (initial noise sampled from a Gaussian) against a golden ticket (a well-chosen, constant initial noise) are on the project website.

# Overview

This repository accompanies the paper *Improving Generative Robot Policies With A Single Noise Vector*.
The performance of a pretrained, frozen diffusion or flow matching policy can be improved with respect to a downstream reward by swapping the sampling of initial noise from the prior distribution (typically isotropic Gaussian) with a well-chosen, constant initial noise input, which we call a **golden ticket**.

Golden Ticket (GT) Search is an episodic, derivative-free policy improvement approach.
Candidate initial noise vectors, which we call **lottery tickets**, are evaluated with policy rollouts, and the ticket with the highest average reward on the downstream task is kept.
The pretrained policy weights stay frozen and no additional network is trained.

## What this release contains

The paper considers random search and the cross-entropy method (CEM), optionally with sequential halving.
This repository implements random search: it samples lottery tickets from a Gaussian, evaluates each of them, and returns the best one.

It provides code and golden tickets for three of the paper's simulated benchmarks, where each uses a different simulator and policy class:

1. <a href="./src/lottery_tickets/franka_sim_lt/README.md">franka_sim cube picking with state-based flow matching policies</a>
2. <a href="./src/lottery_tickets/smolvla_libero/README.md">🤗 LeRobot pretrained 🤗SmolVLA for LIBERO</a>
3. <a href="./src/lottery_tickets/robomimic_dppo_lt/README.md">DPPO + robomimic</a>

Each setup has its own README, with code for running the base policy, generating lottery tickets and evaluating them, along with the golden tickets we found.
Each subfolder may contain other utilities, since each setup serves a different purpose:

<a href="./src/lottery_tickets/franka_sim_lt/README.md">🦾 franka_sim</a> involves a cube picking task with a Franka robot, where the cube randomly spawns in a ~1/2 square meter region in front of the robot.
Our codebase includes an automated way to generate demonstrations, training code for behavior cloning with a flow matching policy on the collected data, and model checkpoints of policies we have already trained.
We also include golden tickets for the checkpoints we provide.
This setup is useful for examining every part of the pipeline (data collection, policy training, and inference) behind a policy with golden tickets.
The model is small, so experiments need little compute.

<a href="./src/lottery_tickets/smolvla_libero/README.md">🤗 SmolVLA + LIBERO</a> uses a pretrained VLA checkpoint taken directly from LeRobot and searches for golden tickets over the LIBERO task suites.
We also include the golden tickets we found, which can be evaluated.
This setup is suited to examining lottery tickets with an open-source VLA in a multi-task setting.
The policy is an off-the-shelf LIBERO checkpoint from LeRobot, so it reflects searching for golden tickets in a model we did not train.

<a href="./src/lottery_tickets/robomimic_dppo_lt/README.md">✨ DPPO for robomimic</a> uses the DPPO robomimic checkpoints that were also used in DSRL.
We provide golden tickets for these policies, and code for generating new lottery tickets and comparing against the base policy.
This is again a model we did not train.

# Getting started

We use some features of the [`uv`](https://docs.astral.sh/uv/) package manager in our `pyproject.toml`.
The easiest way to get started is to install `uv` using [these instructions](https://docs.astral.sh/uv/getting-started/installation/).

You can then install the individual experiment setups using `uv sync --extra $EXPERIMENT_NAME`; see the individual READMEs for more details.