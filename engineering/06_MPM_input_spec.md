# MPM Input Specification

Version: v8  
Layer: Engineering Layer

---

# 1 Purpose

This document defines the input structure required by the
Music Personality Model (MPM) inference system.

MPM predicts listener reward compatibility by combining:

• listener personality  
• track structural signals

These inputs allow the model to simulate how a specific listener
is likely to experience a specific piece of music.

---

# 2 MPM Input Structure

The MPM inference system receives the following inputs:

MPM_input

{

listener_personality  
mpm_signals

}

Each input component is defined in the following sections.

---

# 3 Listener Personality Input

Listener personality represents how the listener allocates
perceptual attention across musical dimensions.

This input is produced by the Personality Layer.

Source:

05_personality_generation.md

Structure:

listener_personality

{

listener_id  

Wm  
Wr  
Wt  
Wa  

}

Where:

Wm = melody perception weight  
Wr = rhythm perception weight  
Wt = timbre perception weight  
Wa = arrangement perception weight  

Constraint:

Wm + Wr + Wt + Wa = 1

These weights influence how strongly the listener responds
to different structural properties of the track.

---

# 4 Default Personality

If listener personality data is unavailable,
the system uses the default personality profile.

Default personality:

listener_id : user0

Wm = 0.32  
Wr = 0.26  
Wt = 0.24  
Wa = 0.18

This profile represents the calibrated reference listener used
during model development.

---

# 5 Track Signal Input

MPM requires structural signals describing the perceptual
characteristics of a music track.

These signals are:

melody_signal  
rhythm_signal  
timbre_signal  
arrangement_signal  

Structure:

mpm_signals

{

melody_signal  
rhythm_signal  
timbre_signal  
arrangement_signal  

}

All signal values should be normalized to the range:

0.0 – 1.0

---

# 6 Signal Generation

MPM signals may be derived from several sources:

• automated audio analysis  
• symbolic music analysis  
• dataset preprocessing  
• feature mapping from external APIs

In the current MPM demo implementation,
signals are derived from **Spotify Audio Features**.

Spotify features are mapped into MPM structural signals
through a signal mapping layer.

Spotify audio features include:

danceability  
energy  
tempo  
loudness  
valence  
speechiness  
acousticness  
instrumentalness

These features are converted into MPM signals before
the inference process begins.

The signal mapping process is defined in:

06b_MPM_signal_mapping.md

---

# 7 Listener State (Optional)

In addition to stable personality traits,
listener evaluation may also be influenced by temporary states.

Examples include:

• attention level  
• fatigue  
• emotional state  
• novelty sensitivity

These variables represent **listener state**, not personality.

The current MPM v8 inference system does not require listener state
as a mandatory input.

Future versions may optionally include:

listener_state

{

attention_level  
novelty_bias  
fatigue_level  

}

---

# 8 Input Validation

Before running the inference pipeline,
the input should satisfy the following conditions:

Listener personality weights must sum to 1.

All required MPM signals must be present.

All signal values should be normalized to the range:

0.0 – 1.0

Missing signal values should be handled through
default values or preprocessing.

---

# 9 Role in MPM Pipeline

The MPM input stage prepares data for the inference engine.

Pipeline structure:

Spotify Audio Features  
↓  
Signal Mapping Layer  
↓  
MPM Signals  
↓  
MPM Inference  
↓  
MPM Output

The inference process itself is defined in:

08_MPM_inference_spec.md

---

# 10 Design Principles

The MPM input system follows several design principles.

Separation of personality and structure

Listener personality represents perceptual weighting,
while track signals represent structural properties.

Modular feature mapping

MPM does not depend on a specific audio analysis system.

Extensibility

Additional structural signals may be added without
changing the personality system.

---

# 11 Summary

MPM inference requires two primary inputs:

listener personality  
track structural signals

Personality defines how the listener allocates attention,
while structural signals describe perceptual properties of the music.

Together these inputs allow MPM to simulate
listener-specific reward generation.
