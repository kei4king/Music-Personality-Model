# MPM Inference Specification

Version: v8
Layer: Engineering Layer

---

# 1 Purpose

This document defines the inference logic used by the
Music Personality Model (MPM).

The inference system estimates how a listener will experience
a piece of music by simulating the interaction between:

• structural signals  
• listener personality weights  
• prediction dynamics  
• reward generation  
• engagement failure mechanisms  

The goal of the inference system is to estimate:

predicted_reward  
recommendation_safety

The final recommendation decision is based on these outputs.

---

# 2 Inference Pipeline

The MPM inference process follows the pipeline below:

Music Feature Input
↓
Signal Mapping
↓
Structural Signal Evaluation
↓
Perceptual Failure Detection
↓
Prediction Modeling
↓
Reward Generation
↓
Failure Penalty Integration
↓
final_reward = base_reward − failure_penalty
↓
Reward Bank Accumulation
↓
Recommendation Safety Evaluation

Signal mapping is defined in:

07_MPM_signal_mapping.md

Variables are defined in:

08_MPM_variable_spec.md

---

# 3 Structural Signal Evaluation

The signal mapping layer produces four primary perception signals:

melody_signal  
rhythm_signal  
timbre_signal  
arrangement_signal  

These signals represent the structural properties of the track.

Each signal is normalized to the range:

0.0 – 1.0

---

# Structural Score

Structural signals are aggregated using listener perception weights.

structural_score =

(Wm × melody_signal) +
(Wr × rhythm_signal) +
(Wt × timbre_signal) +
(Wa × arrangement_signal)

Range:

1 ≤ structural_score ≤ 7

---

# 4 Personality Weight Interaction

Listener personality determines how strongly each signal
influences the listening experience.

The listener is represented by four perception weights:

Wm — melody sensitivity  
Wr — rhythm sensitivity  
Wt — timbre sensitivity  
Wa — arrangement sensitivity  

Structural signals are transformed into listener-specific
perception strength:

melody_perception = melody_signal × Wm  
rhythm_perception = rhythm_signal × Wr  
timbre_perception = timbre_signal × Wt  
arrangement_perception = arrangement_signal × Wa  

These values represent the effective perceptual influence
of each structural dimension.

---

# 5 Prediction Modeling

Prediction modeling estimates how predictable the music
structure is for the listener.

Prediction variables include:

rhythm_predictability  
melody_predictability  
arrangement_predictability  

Prediction success produces **reward pulses**.

Prediction failure produces **prediction_error**.

Repeated prediction error suppresses reward accumulation.

---

# Score Calibration

MPM predicted reward is mapped to the 1–7 rating scale
using a calibrated transformation.

In MPM, score 4 represents structural compatibility
rather than reward.

Score mapping:

score = 4 + 3 × sigmoid(k × (reward − r0))

Recommended parameters:

r0 = 0.52
k = 6.5

---

# 6 Reward Generation

Reward pulses represent successful confirmation of
listener expectations.

Typical reward triggers include:

• melodic resolution  
• motif repetition  
• rhythmic drops  
• phrase closure  
• harmonic cadence  

Each reward event generates a **reward_pulse**.

Reward pulses accumulate during listening.

---

# Reward Efficiency

Reward efficiency evaluates how quickly reward events appear relative
to the amount of musical buildup.

reward_efficiency =

reward_peak_intensity /
(idle_intro_time + tension_build_duration + 1)

reward_bonus =

reward_efficiency × 1.2

---

# 7 Reward Density Normalization

Reward density measures how frequently reward pulses
occur during listening.

To ensure fair comparison across tracks of different lengths,
MPM normalizes reward density by time.

Previous approximation:

reward_density = reward_pulse / track

Updated normalization:

track_duration_minutes = track_duration_seconds / 60

reward_density = reward_pulse / track_duration_minutes

This prevents long compositions from being artificially
penalized due to longer duration.

---

# Reward Variance

Reward timing stability influences listener engagement.

reward_events = [t1, t2, t3 ...]

interval_i = t(i+1) − t(i)

μ = mean(interval)

σ = standard_deviation(interval)

reward_variance = σ / μ

variance_factor =

1 / (1 + reward_variance)

---

# 8 Failure Detection

MPM models two categories of failure mechanisms:

perceptual failures  
prediction failures

---

### Perceptual Failure Detection

Early perceptual failures may occur before higher-level
prediction processes stabilize.

MPM estimates perceptual failure risk using three proxy variables:

timbre_harshness  
vocal_density  
structure_instability  

These variables are combined to estimate a perceptual
failure penalty.

failure_penalty =

α × timbre_harshness  
+ β × vocal_density  
+ γ × structure_instability

These include:

timbre_gate  
chaos_penalty  
attention_decay  
intro_latency  
primary_reward_channel_collapse  

These variables represent different types of listening failure.

For example:

timbre_gate occurs when problematic sonic texture
captures attention and blocks other perceptual signals.

primary_reward_channel_collapse occurs when the
listener's dominant reward channel fails to stabilize.

Failure mechanisms reduce effective reward accumulation.

---

# Structural Chaos

Structural chaos represents instability in musical structure.

structural_chaos =

0.5 × motif_replacement_rate +
0.3 × rhythm_discontinuity +
0.2 × structure_switching

chaos_factor =

1 − α × structural_chaos

Recommended value:

α ≈ 0.5

Range:

0.5 ≤ chaos_factor ≤ 1

---

# 9 Structural Mode Detection

Some tracks follow structural patterns that require
special interpretation.

MPM uses proxy signals derived from Spotify Audio Features
to detect structural modes.

Example:

orchestral_index =
instrumentalness × (1 − danceability) × (1 − speechiness)

If:

orchestral_index > 0.60

the track enters **orchestral structural mode**.

In this mode, arrangement and melodic signals may
dominate reward generation.

---

# 10 Reward Bank Accumulation

MPM simulates the temporal experience of listening by
accumulating reward signals across multiple timeline phases.

Reward accumulation is stored in four reward banks:

intro_reward_bank  
early_reward_bank  
middle_reward_bank  
late_reward_bank  

Each bank represents reward generated during a
different phase of the track.

---

# 11 Reward Bank Segmentation

Reward banks are evaluated using deterministic
time-based segmentation.

Intro phase

0 – 30 seconds

Remaining track duration is divided equally
into three phases.

remaining_time = total_duration − 30

phase_length = remaining_time / 3

early phase

30 – (30 + phase_length)

middle phase

(30 + phase_length) – (30 + 2 × phase_length)

late phase

(30 + 2 × phase_length) – total_duration

Reward signals generated during each window
contribute to the corresponding reward bank.

---

# 12 Intro Reward Bank Check (IRBC)

The intro reward bank is used to evaluate
early listener engagement.

IRBC determines whether sufficient reward
appears during the intro phase.

IRBC failure also reduces predicted_score.

If intro_reward_bank < threshold:

predicted_score = predicted_score − IRBC_penalty

Recommended value:

IRBC_penalty ≈ 1.0

the system assumes the track has weak
early engagement.

This reduces recommendation safety.

Tracks with weak intros may still produce
high total reward but are considered risky
recommendations.

IRBC failure also reduces predicted_score.

---

# 13 Primary Reward Channel Stability (PRCS)

Definition

PRCS = dominant_perception − second_dominant_perception

Where perception channels are:

melody_perception  
rhythm_perception  
timbre_perception  
arrangement_perception

Interpretation

PRCS measures whether one structural channel clearly
dominates the listening experience.

Effect

Reward amplification occurs when a dominant channel exists.

Reward adjustment:

reward = reward × (0.6 + 0.4 × PRCS)

---

# 14 Final Score Model

The final predicted score is computed as:

final_score =

(
structural_score
+ reward_bonus
)
× variance_factor
× chaos_factor

− intro_penalty
− timbre_penalty
− drought_penalty

Clamp:

1 ≤ final_score ≤ 7

---

# 15 Recommendation Safety Evaluation

Final recommendation decisions are based on:

predicted_reward  
recommend_score

recommend_score integrates multiple risk factors:

• IRBC failure  
• timbre_gate  
• chaos_penalty  
• long intro latency  
• reward drought

Tracks with low recommend_score should not be
recommended even if predicted reward is moderate.

---

# 16 Output Metrics

The MPM inference system produces two main outputs.

predicted_score

Represents expected listener enjoyment.

recommend_score

Represents recommendation safety.

Example interpretation:

High predicted_score + low recommend_score

→ good music but risky recommendation.

High predicted_score + high recommend_score

→ strong recommendation candidate.

---

# 17 Summary

The MPM inference system simulates the human
music listening process by modeling:

structural perception  
prediction dynamics  
reward generation  
engagement failure  
temporal reward accumulation  

By combining these mechanisms with listener
personality weights, MPM predicts how a listener
will experience musical structure.

This allows the system to estimate both:

listener reward compatibility  
recommendation safety
