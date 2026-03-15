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
Prediction Modeling  
↓  
Reward Generation  
↓  
Failure Detection  
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

# 8 Failure Detection

MPM models several mechanisms that may suppress reward
or disrupt engagement.

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

If:

intro_reward_bank < threshold

the system assumes the track has weak
early engagement.

This reduces recommendation safety.

Tracks with weak intros may still produce
high total reward but are considered risky
recommendations.

---

# 13 Recommendation Safety Evaluation

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

# 14 Output Metrics

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

# 15 Summary

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
