# MPM Signal Mapping Specification

Version: v8  
Layer: Engineering Layer

---

# 1 Purpose

This document defines how external music features
are converted into MPM structural signals.

In the current MPM demo implementation,
signals are derived from **Spotify Audio Features**.

These features are mapped into four core MPM signals:

melody_signal  
rhythm_signal  
timbre_signal  
arrangement_signal

---

# 2 Spotify Audio Features

Spotify provides the following audio features:

danceability  
energy  
tempo  
loudness  
valence  
speechiness  
acousticness  
instrumentalness

These values are normalized to the range:

0.0 – 1.0

except:

tempo  
loudness

which require normalization.

---

# 3 Normalization

tempo normalization

tempo_norm = tempo / 200

loudness normalization

loudness_norm = (loudness + 60) / 60

These transformations convert the variables
into the range 0.0 – 1.0.

---

# 4 Melody Signal Mapping

melody_signal is estimated using the following mapping:

melody_signal =
w1 × melody_strength
+ w2 × melody_resolution
+ w3 × melody_predictability

melody_strength

Measures prominence of melodic contour.

melody_resolution

Measures strength of melodic closure.

melody_predictability

Measures probability of predicted pitch continuation.

Interpretation:

Valence reflects melodic emotional expression.

Instrumentalness increases probability of melodic content.

Speechiness reduces melodic clarity.

---

# 5 Rhythm Signal Mapping

rhythm_signal is estimated as:

rhythm_signal =

0.5 × danceability  
+ 0.3 × tempo_norm  
+ 0.2 × energy

Interpretation:

Danceability reflects rhythmic engagement.

Tempo contributes to rhythmic drive.

Energy supports rhythmic propulsion.

---

# 6 Timbre Signal Mapping

timbre_signal is estimated as:

timbre_signal =

0.4 × energy  
+ 0.3 × (1 − acousticness)  
+ 0.3 × loudness_norm

Interpretation:

Energy reflects spectral intensity.

Acousticness indicates timbral softness.

Loudness reflects sonic density.

---

# 7 Arrangement Signal Mapping

arrangement_signal is estimated as:

arrangement_signal =

0.5 × (1 − instrumentalness)  
+ 0.3 × speechiness  
+ 0.2 × energy

Interpretation:

Vocal presence contributes to structural segmentation.

Speechiness indicates lyrical structure.

Energy supports structural layering.

---

# 8 Signal Range

All generated signals must be clamped to:

0.0 – 1.0

If values exceed the range,
they should be clipped.

---

# 9 Structural Mode Detection (Post Mapping)

Certain structural modes are inferred from
Spotify Audio Features after signal mapping.

Example:

orchestral_index =
instrumentalness × (1 − danceability) × (1 − speechiness)

These proxy signals allow MPM to approximate
structural categories without explicit genre labels.

---

## Structural Feature Normalization

### melody

Normalized melodic prominence score derived from pitch variation
and motif repetition.

Range:
0 – 1

### rhythm

Normalized rhythmic regularity score derived from beat stability
and groove predictability.

Range:
0 – 1

### timbre

Normalized timbral richness and texture quality score.

Range:
0 – 1

### arrangement

Normalized arrangement complexity and structural layering score.

Range:
0 – 1

---

# 10 Role in MPM Pipeline

The signal mapping layer converts low-level audio features
into perceptual signals used by MPM.

Pipeline:

Spotify Audio Features  
↓  
Signal Mapping Layer  
↓  
MPM Structural Signals  
↓  
MPM Inference  
↓  
MPM Output

---

# 11 Summary

The signal mapping layer allows MPM to operate
without requiring full audio structure analysis.

By converting Spotify features into structural signals,
the model can simulate perceptual listening behavior
using available music metadata.

---

# 12 Perceptual Failure Proxy Signals

In addition to structural signals, MPM derives several
proxy variables used to estimate perceptual failures.

These variables approximate failure mechanisms using
Spotify Audio Features.

timbre_harshness ≈ energy × loudness_norm

vocal_density ≈ speechiness × (1 − instrumentalness)

structure_instability ≈ (1 − danceability) × tempo_norm

These signals are not direct acoustic measurements but
approximate perceptual failure risk using available metadata.

The resulting variables are used during the failure
detection stage of the MPM inference pipeline.
