# MPM Variable Specification

Version: v8  
Layer: Engineering Layer

---

# 1 Purpose

This document defines the engineering variables used by the
Music Personality Model (MPM).

These variables describe the structural signals, prediction
dynamics, reward generation, and failure mechanisms involved
in music listening.

The variables defined here are used by the MPM inference
pipeline to estimate listener reward compatibility.

Mathematical inference logic is defined separately in:

09_MPM_inference_spec.md

---

# 2 Variable Categories

MPM variables are grouped into five categories:

perception variables  
prediction variables  
reward variables  
failure variables  
recommendation variables  

Each category corresponds to a different stage of the
listening process.

---

# 3 Perception Variables

Perception variables represent the structural signals
detected during listening.

These signals correspond to the perceptual hierarchy
described in the MPM theoretical framework.

---

## timbre_signal

Description

Represents sonic texture characteristics of the track.

Examples:

• harshness  
• spectral brightness  
• sound density  
• texture stability  

Role

Timbre is the earliest perceptual signal and can strongly
influence listener attention.

---

## rhythm_signal

Description

Represents rhythmic clarity and propulsion.

Examples:

• beat stability  
• groove strength  
• tempo clarity  

Role

Strong rhythmic signals sustain engagement and support
prediction stability.

---

## melody_signal

Description

Represents melodic structure within the track.

Examples:

• motif strength  
• melodic contour clarity  
• melodic repetition  

Role

Melody is a major reward channel for many listeners.

Melodic confirmation may produce strong reward pulses.

---

## arrangement_signal

Description

Represents structural organization of the track.

Examples:

• section transitions  
• phrase development  
• structural variation  

Role

Arrangement influences predictability and structural stability.

---

# 4 Prediction Variables

Prediction variables describe how predictable the musical
structure is to the listener.

Prediction success is a key driver of reward generation.

---

## rhythm_predictability

Measures how predictable rhythmic patterns are.

Higher predictability supports rhythmic engagement.

---

## melody_predictability

Measures how predictable melodic continuation is.

Melodic predictability contributes to reward pulses.

---

## arrangement_predictability

Measures predictability of structural progression.

Low predictability may increase prediction error.

---

## prediction_error

Description

Represents unexpected musical events that violate listener
predictions.

Examples:

• chaotic structural transitions  
• unstable rhythmic patterns  
• abrupt melodic deviation  

Effect

Prediction error suppresses reward accumulation.

Repeated prediction error reduces overall enjoyment.

---

# 5 Reward Variables

Reward variables represent the positive reinforcement
generated during listening.

---

## reward_pulse

Description

Represents a moment of successful structural prediction.

Typical triggers include:

• melodic resolution  
• motif repetition  
• harmonic cadence  
• phrase closure  
• rhythmic drop  

Reward pulses accumulate over time to produce musical enjoyment.

---

## reward_latency

Description

Represents the time of the first stable reward pulse.

Tracks with very late reward latency may struggle to
maintain listener engagement.

---

## reward_density

Description

Represents how frequently reward pulses occur during the track.

High reward density indicates frequent structural confirmation.

---

## reward_peak_intensity

Description

Represents the strongest reward pulse within the track.

Tracks with strong reward peaks often produce higher final ratings.

---

## reward_variance

Description

Measures variability of reward pulses over time.

Balanced variance may sustain engagement better than
flat reward distributions.

---

## reward_drought

Description

Represents long periods without reward pulses.

Typical causes include:

• static passages  
• prolonged build-ups  
• repetitive loops without payoff  

Effect

Reward drought gradually reduces engagement.

---

# 6 Failure Variables

Failure variables represent mechanisms that suppress reward
or disrupt engagement.

---

## perceptual_failure_index

Description

Represents low-level perceptual failures that may disrupt
listener engagement before higher-level prediction processes
stabilize.

These failures are detected using proxy variables derived
from Spotify audio features.

Sub-components

• timbre_harshness  
• vocal_density  
• structure_instability  

Role

Perceptual failures may suppress reward acquisition
by disrupting early perceptual processing.

These variables contribute to the overall failure_penalty
used in the MPM inference stage.

---

## timbre_harshness

Description

Represents harsh or piercing sonic textures that may
capture listener attention and disrupt melodic or rhythmic
processing.

Typical causes include:

• extreme high-frequency energy  
• distorted synthetic textures  
• excessively loud spectral peaks  

Effect

High timbre harshness increases the perceptual failure index
and may trigger timbre-related engagement suppression.

---

## vocal_density

Description

Represents excessive vocal stacking or chant-like repetition
that may reduce structural clarity.

Typical causes include:

• dense vocal layering  
• repetitive chant structures  
• overly compressed vocal textures  

Effect

High vocal density may reduce perceived structural clarity
and weaken melodic reward signals.

---

## structure_instability

Description

Represents unstable structural progression that prevents
listeners from forming reliable predictions.

Examples include:

• abrupt section changes  
• chaotic arrangement transitions  
• highly irregular rhythmic shifts  

Effect

Structural instability increases prediction error and
reduces reward accumulation.

---

## timbre_gate

Description

Represents attention disruption caused by problematic timbre.

When sonic texture strongly captures listener attention,
the listener may struggle to process other perceptual
dimensions such as melody, rhythm, or arrangement.

Effect

Timbre gate reduces effective reward acquisition
from other structural channels.

---

## timbre_shock_index (TSI)

Represents sudden loudness spikes that may trigger
a timbre gate event.

Definition:

TSI ≈ estimated sudden loudness change

In the current demo implementation,
TSI is approximated using proxy signals
derived from Spotify audio features.

Role:

Detect unexpected sonic shock events
that may cause listener discomfort.

TSI may trigger the timbre_gate mechanism.

---

## orchestral_index

Detects orchestral structural mode

Definition:

orchestral_index =
instrumentalness
× (1 − danceability)
× (1 − speechiness)

Tracks with high orchestral_index
are treated as orchestral structural mode.

Typical threshold used in the demo:

orchestral_index > 0.60

Role:

Correct structural signal interpretation
for instrumental soundtrack music.

---

## chaos_penalty

Description

Represents structural instability that disrupts prediction.

Examples:

• chaotic arrangement  
• unstable rhythmic transitions  
• unpredictable structural shifts  

Effect

High chaos suppresses reward accumulation.

---

## primary_reward_channel_collapse

Description

Represents failure of the listener's dominant reward pathway.

Example:

A melody-focused listener may fail to obtain reward
if melodic payoff never stabilizes.

Effect

Reward accumulation collapses even if other signals
remain structurally compatible.

---

## attention_decay

Description

Represents gradual loss of engagement when stimulation
remains weak for too long.

Example:

long intros with minimal structural reward.

Effect

Attention decay reduces prediction accuracy and
reward detection.

---

## intro_latency

Description

Represents delay before meaningful reward signals appear.

Long intro latency may reduce listener engagement.

Intro latency influences the size of the intro reward bank.

---

## intro_latency_tolerance

Definition

Maximum delay before the listener expects the first reward signal.

If reward_latency exceeds this tolerance,
a latency penalty is applied.

Example (user0)

tolerance ≈ 25 seconds

---

# 7 Recommendation Variables

Recommendation variables are used during the final
recommendation decision stage.

---

## reward_bank

Definition

Represents accumulated reward signals during listening.

To simulate the human concept of listening to music over time,
the reward bank will consist of the following four parts:

intro_reward_bank  
early_reward_bank  
middle_reward_bank  
late_reward_bank

Reward bank integrates:

• reward pulses  
• prediction error  
• reward drought damage  
• timbre gate damage  

Reward bank may be evaluated across different time windows,
including intro reward evaluation.

---

## IRBC

Intro Reward Bank Check

Description

Evaluates whether sufficient reward has accumulated during
the intro section of the track.

If intro reward bank is below the required threshold,
the system assumes the track lacks early engagement.

Effect

IRBC failure reduces recommendation safety.

Tracks with weak early reward become risky to recommend.

---

## recommend_score

Description

Represents recommendation safety.

Recommend score integrates predicted reward with
early failure mechanisms.

These include:

• IRBC  
• timbre gate  
• chaos penalty  
• long intro penalty  

Tracks with low recommend score should not be recommended
even if predicted reward is moderate.

---

# 8 Personality Interaction

MPM variables interact with listener personality weights.

Listener weights determine how strongly each structural
signal influences reward prediction.

Example

melody_signal × Wm  
rhythm_signal × Wr  
timbre_signal × Wt  
arrangement_signal × Wa  

These interactions allow MPM to simulate
listener-specific reward generation.

---

# 9 Variable Scope

Variables defined here are engineering variables used by the
MPM inference system.

They are not listener personality dimensions.

Personality remains limited to the four core perception axes:

melody  
rhythm  
timbre  
arrangement

---

# 10 Summary

The MPM variable system describes structural perception,
prediction dynamics, reward generation, engagement failure,
and recommendation safety mechanisms involved in music listening.

These variables form the foundation of the MPM inference
engine that predicts listener reward compatibility.