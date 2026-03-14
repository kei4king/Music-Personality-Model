# MPM Output Specification

Version: v8  
Layer: Engineering Layer

---

# 1 Purpose

This document defines the output structure produced by the
Music Personality Model (MPM) inference system.

The output describes the predicted listening outcome for a
specific listener–track pair.

MPM outputs three primary values:

predicted score  
confidence  
recommend score

These values summarize the model’s prediction about:

listener enjoyment  
prediction certainty  
recommendation safety

---

# 2 Output Structure

The inference system returns the following structure:

MPM_output

{

track_id

predicted_score

confidence

recommend_score

}

Optional explanatory information may also be included.

---

# 3 Predicted Score

Description

Predicted score represents the expected enjoyment level of the
track for the listener.

The score estimates how rewarding the track will be if the listener
fully experiences it.

Scale

MPM uses a **1–7 rating scale**.

1 = strong rejection  
2 = dislike  
3 = weak compatibility  
4 = neutral compatibility  
5 = moderate enjoyment  
6 = strong enjoyment  
7 = exceptional reward

Interpretation

Scores around 4 represent tracks that are structurally compatible
but do not generate strong reward.

Scores above 6 indicate tracks likely to produce strong reward
responses.

---

# 4 Confidence

Description

Confidence represents the model’s certainty about the predicted score.

Confidence reflects how stable the reward prediction is given the
structural signals of the track.

Confidence does **not** represent the probability that the user will
like the track.

Instead it represents:

prediction stability.

Scale

Confidence is expressed as a percentage:

0% – 100%

Interpretation

High confidence

The model is confident that the predicted score is accurate.

Low confidence

The track contains structural uncertainty and prediction is less stable.

Example

predicted_score = 4  
confidence = 90%

Interpretation:

The model is highly confident that the track will produce
neutral compatibility rather than strong reward.

---

# 5 Recommend Score

Description

Recommend score represents the **safety of recommending the track**.

This value considers:

predicted reward  
early failure risk  
reward stability

Even if a track has moderate predicted reward, it may receive a low
recommend score if early failure risk is high.

Scale

Recommend score ranges from:

0.0 – 1.0

Interpretation

0.0 – 0.3

Unsafe recommendation  
High failure risk

0.3 – 0.6

Moderate recommendation quality

0.6 – 1.0

Strong recommendation candidate

Recommend score is the primary value used by the
recommendation system to select tracks.

---

# 6 Example Output

Example output structure:

MPM_output

{

track_id: "M83 - Midnight City"

predicted_score: 4.3

confidence: 93%

recommend_score: 0.41

}

Interpretation

The track is structurally compatible but does not generate strong reward.

Recommendation safety is moderate due to limited reward potential.

---

# 7 Recommendation Decision

Recommendation systems may apply thresholds to select tracks.

Example policy:

recommend_score > 0.6

→ candidate recommendation

recommend_score < 0.3

→ filtered from recommendation list

These thresholds may be adjusted depending on
application requirements.

---

# 8 Optional Explanation Output

For debugging or interpretability, the system may optionally
return explanation fields.

Example:

reason

{

reward_density

reward_peak_intensity

chaos_penalty

intro_latency

}

These fields help developers understand why the model
produced a particular prediction.

These fields are optional and are not required for the
standard inference output.

---

# 9 Role in MPM Pipeline

The output stage is the final step of the MPM pipeline.

Full pipeline:

MPM_input  
↓  
variable processing  
↓  
MPM_inference  
↓  
MPM_output

The output structure defined here is the interface exposed
to downstream systems such as recommendation engines,
analysis tools, or evaluation experiments.

---

# 10 Summary

MPM produces three core outputs for each listener–track pair:

predicted_score

confidence

recommend_score

These outputs allow downstream systems to evaluate both:

expected listener enjoyment  
recommendation safety

without requiring direct knowledge of the internal
MPM inference process.
