# MPM Variable Specification

Version: v8  
Layer: Engineering Layer

---

# 1 Purpose

This document defines the structural variables used by the
Music Personality Model (MPM).

These variables describe perceptual signals,
prediction dynamics, reward generation,
and failure mechanisms during music listening.

---

# 2 Variable Categories

MPM variables are divided into the following categories:

perception variables  
prediction variables  
reward variables  
failure variables  
recommendation variables

Each category corresponds to a different stage of the listening process.

---

# 3 Perception Variables

Perception variables represent structural signals
detected by the listener.

These signals form the foundation of reward generation.

---

## melody_signal

Represents the presence and clarity of melodic structure.

Examples:

• motif repetition  
• melodic contour  
• melodic stability  

Role:

Melody confirmation can generate strong reward pulses.

---

## rhythm_signal

Represents rhythmic clarity and temporal propulsion.

Examples:

• beat stability  
• groove strength  
• tempo clarity  

Role:

Rhythm contributes to sustained engagement.

---

## timbre_signal

Represents sonic texture characteristics.

Examples:

• spectral brightness  
• sound density  
• texture stability  

Role:

Timbre is typically the earliest perceptual signal.

Extreme timbre characteristics may trigger early rejection.

---

## arrangement_signal

Represents structural organization of the track.

Examples:

• phrase development  
• section transitions  
• structural variation  

Role:

Arrangement affects predictability and structural stability.

---

# 4 Prediction Variables

Prediction variables describe how predictable
the music structure is to the listener.

---

## rhythm_predictability

Measures predictability of rhythmic patterns.

---

## melody_predictability

Measures predictability of melodic continuation.

---

## arrangement_predictability

Measures predictability of structural progression.

Prediction confirmation generates reward pulses.

Prediction failure produces prediction error.

---

# 5 Reward Variables

Reward variables describe positive reinforcement
generated during listening.

---

## reward_density

Frequency of reward pulses.

---

## reward_peak_intensity

Strength of the strongest reward event.

---

## reward_variance

Distribution variability of reward pulses.

Balanced reward variance helps maintain engagement.

---

# 6 Failure Variables

Failure variables represent mechanisms
that suppress reward generation.

---

## timbre_veto

Immediate rejection triggered by intolerable timbre.

---

## chaos_penalty

Structural instability that disrupts prediction.

Examples:

• chaotic arrangement  
• unstable rhythm transitions  
• unpredictable structure

---

## intro_latency

Delay before meaningful reward appears.

Long intro latency may reduce listener engagement.

---

# 7 Recommendation Variables

Recommendation variables are used during
final recommendation decisions.

---

## reward_bank

Accumulated reward potential.

---

## IRBC

Initial Reward Breakpoint Confidence.

Measures probability that reward stabilizes early.

---

## recommend_score

Represents recommendation safety.

Even tracks with moderate reward may receive low
recommend scores if early failure risk is high.

---

# 8 Personality Interaction

MPM variables interact with listener personality weights.

Example:

melody_signal × Wm  
rhythm_signal × Wr  
timbre_signal × Wt  
arrangement_signal × Wa

These interactions simulate listener-specific perception.

---

# 9 Signal Source

MPM structural signals may be derived from
low-level audio features.

In the current MPM demo implementation,
signals are derived from Spotify Audio Features.

Spotify features are mapped into MPM signals
through the signal mapping layer defined in:

06b_MPM_signal_mapping.md

---

# 10 Summary

MPM variables describe structural perception,
prediction dynamics, reward generation,
and failure mechanisms involved in music listening.

These variables form the basis for the MPM inference engine.
