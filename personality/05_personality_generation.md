# Listener Personality Generation

Version: v8  
Layer: Personality Layer

---

# 1 Purpose

This document defines how LSQ scoring results are converted into a
listener personality object used by the Music Personality Model (MPM).

The personality object represents the listener’s **structural perception
distribution**, which determines how attention is allocated across
musical dimensions during listening.

The personality object is a stable representation of the listener’s
structural listening tendencies.

It serves as a primary input for the MPM inference system.

---

# 2 Personality Generation Pipeline

Listener personality is generated through the following pipeline:

LSQ questionnaire  
↓  
LSQ scoring  
↓  
perception weight generation  
↓  
listener personality object

The LSQ scoring stage produces normalized perception weights.

These weights are then assembled into a personality structure.

---

# 3 Personality Structure

The MPM listener personality is defined by four perception weights:

melody  
rhythm  
timbre  
arrangement

These correspond to the four primary structural perception dimensions
defined in the MPM theoretical framework.

The personality object is represented as:

listener_personality

{

Wm : melody perception weight  
Wr : rhythm perception weight  
Wt : timbre perception weight  
Wa : arrangement perception weight  

}

The weights satisfy:

Wm + Wr + Wt + Wa = 1

---

# 4 Interpretation

The perception weights represent how strongly a listener naturally
allocates attention to each musical dimension.

Higher weights indicate stronger perceptual sensitivity.

Example:

Wm = 0.34  
Wr = 0.27  
Wt = 0.22  
Wa = 0.17  

This listener is strongly melody-oriented.

The MPM inference system will therefore place greater importance on
melodic structure when evaluating tracks.

---

# 5 Listener Personality Types (Optional Classification)

For interpretability, listeners may optionally be categorized into
broad personality types based on dominant perception dimensions.

Example categories:

Melody-driven listener  
Rhythm-driven listener  
Timbre-driven listener  
Structure-driven listener

This classification is optional and does not affect the inference
mechanism.

The MPM inference engine always operates on the continuous weight vector.

Personality classification is optional and must not influence
the inference calculation.

Only the numerical perception weights
(Wm, Wr, Wt, Wa) may affect prediction.

---

# 6 Default Personality (user0)

The current MPM reference listener is **user0**.

This personality profile was derived from calibration experiments
conducted during model development.

The user0 profile represents a listener with strong sensitivity to:

• melodic structure  
• rhythmic propulsion  
• sonic texture

and moderate sensitivity to arrangement structure.

Default personality weights:

listener_id : user0

Wm = 0.32
Wr = 0.24
Wt = 0.28
Wa = 0.16

This personality profile is used as the **default personality**
in the MPM demo environment.

If no LSQ responses are available, the system falls back to
the user0 personality.

---

# 7 Role in MPM Pipeline

The listener personality object is used as an input to the MPM
inference pipeline.

Inference input structure:

listener_personality  
+  
track structural features

↓

MPM inference

↓

predicted reward compatibility

↓

recommendation decision

---

# 8 Personality Stability

Listener personality is considered relatively stable across listening
sessions.

However, personality may gradually adapt over time through behavioral
learning signals.

Future system versions may refine personality using:

• listening history  
• skip behavior  
• replay frequency  
• reinforcement learning signals

These updates should adjust the perception weights gradually rather
than replacing them abruptly.

---

# 9 Design Principles

The MPM personality system follows several design principles.

Low dimensionality

Personality is represented using only four primary perception dimensions.

Cold start compatibility

Personality can be estimated quickly using a short questionnaire.

Interpretability

The personality weights directly correspond to recognizable
music perception tendencies.

Separation from engineering variables

Detailed structural variables used in the inference layer should not
automatically expand the personality dimension space.

---

# 10 Summary

The listener personality object represents the listener’s structural
attention distribution across music perception dimensions.

Core dimensions:

melody  
rhythm  
timbre  
arrangement

The personality object is derived from LSQ scoring results and serves as
a primary input to the MPM inference system.

Default personality (user0) is used when no questionnaire data is
available.
