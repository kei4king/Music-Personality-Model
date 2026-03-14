# LSQ Scoring Specification

Version: v8  
Layer: Personality Layer

---

# 1 Purpose

This document defines how responses from the Listening Style
Questionnaire (LSQ) are converted into listener perception weights.

The LSQ scoring process transforms raw questionnaire answers into
a structural perception profile used by the Music Personality Model (MPM).

The scoring process consists of three stages:

1. dimension score computation
2. normalization
3. perception weight generation

The resulting weights represent how strongly the listener naturally
attends to each musical dimension.

---

# 2 LSQ Dimensions

The LSQ measures four primary perception dimensions.

melody  
rhythm  
timbre  
arrangement

Each dimension is estimated using multiple questionnaire items.

---

# 3 Dimension Score Computation

Raw responses are first aggregated into dimension scores.

Each question uses a 1–7 Likert scale.

Dimension scores are computed as the mean of the corresponding items.

---

## Melody Score

Questions:

Q1  
Q2  
Q3

Computation:

melody_score = mean(Q1, Q2, Q3)

---

## Rhythm Score

Questions:

Q4  
Q5  
Q6

Computation:

rhythm_score = mean(Q4, Q5, Q6)

---

## Arrangement Score

Questions:

Q7  
Q8  
Q9

Computation:

arrangement_score = mean(Q7, Q8, Q9)

---

## Timbre Score

Questions:

Q10  
Q11  
Q12

Computation:

timbre_score = mean(Q10, Q11, Q12)

---

# 4 Score Vector

After aggregation, the listener has a dimension score vector:

[ melody_score  
  rhythm_score  
  timbre_score  
  arrangement_score ]

These scores represent the relative perceptual importance
of each musical dimension for the listener.

However, they are not yet normalized.

---

# 5 Normalization

To produce perception weights suitable for the MPM inference system,
dimension scores are normalized.

First compute the total score:

total_score =
melody_score
+ rhythm_score
+ timbre_score
+ arrangement_score

Then compute normalized weights:

Wm = melody_score / total_score

Wr = rhythm_score / total_score

Wt = timbre_score / total_score

Wa = arrangement_score / total_score

---

# 6 Resulting Perception Weights

The normalized weights represent the listener's perceptual attention
distribution across musical dimensions.

The resulting weight vector is:

[ Wm, Wr, Wt, Wa ]

Where:

Wm = melody perception weight  
Wr = rhythm perception weight  
Wt = timbre perception weight  
Wa = arrangement perception weight

The weights satisfy:

Wm + Wr + Wt + Wa = 1

---

# 7 Interpretation

Higher weight values indicate that the listener tends to allocate
more perceptual attention to that structural dimension during listening.

Example:

Wm = 0.32  
Wr = 0.26  
Wt = 0.24  
Wa = 0.18

This listener is more sensitive to melodic structure than to
arrangement structure.

---

# 8 Role in Personality Generation

The normalized perception weights produced by the LSQ scoring
process serve as the primary input to the personality generation stage.

Pipeline:

LSQ questionnaire  
↓  
LSQ scoring  
↓  
personality generation  
↓  
MPM inference

The LSQ scoring stage produces only the normalized perception weights.

The construction of the full listener personality object is defined
in the personality generation specification.

---

# 9 Design Rationale

The LSQ scoring system intentionally keeps the personality layer
low-dimensional.

Only the four core perceptual dimensions are represented:

melody  
rhythm  
timbre  
arrangement

Engineering-level variables used during inference may be more detailed,
but they should not automatically expand the personality dimensions.

This design keeps personality estimation simple and robust
while allowing the inference system to remain structurally expressive.

---

# 10 Future Extensions

Future versions of the system may refine personality estimation using:

• behavioral listening history  
• skip probability  
• replay behavior  
• reinforcement learning signals

However, these extensions should refine the same core perception weights
rather than replacing them entirely.
