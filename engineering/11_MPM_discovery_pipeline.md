# MPM Discovery Pipeline

Version: v8  
Layer: Engineering Layer

---

# 1 Purpose

This document defines the discovery pipeline used to generate
candidate tracks for the Music Personality Model (MPM).

MPM itself is a **reward prediction model**.

To function as a recommendation system,
MPM requires a discovery pipeline that supplies
candidate tracks for evaluation.

The discovery pipeline performs:

candidate generation  
candidate filtering  
MPM scoring  
recommendation selection

---

# 2 Discovery System Overview

The full recommendation architecture is:

Music Universe
↓
Candidate Generation
↓
Candidate Filtering
↓
MPM Scoring
↓
Recommendation Safety Filter
↓
Final Recommendations

MPM operates primarily in the **scoring stage**.

---

# 3 Music Universe

The music universe represents the total set of available tracks.

In the demo implementation,
the universe is accessed through the Spotify API.

Example sources:

Spotify search API  
Spotify recommendations API  
Spotify playlist API

The discovery pipeline retrieves a subset of tracks
from the music universe as candidate tracks.

---

# 4 Candidate Generation

Candidate generation produces a pool of tracks
to be evaluated by the MPM scoring system.

Typical candidate sources include:

seed tracks  
artist similarity  
playlist sampling  
random exploration

Example candidate pool size:

50–200 tracks

Candidate generation focuses on **coverage** rather than accuracy.

Accuracy is handled by the MPM scoring stage.

---

# 5 Candidate Filtering

Before running MPM inference,
basic filtering may remove unsuitable tracks.

Example filters:

missing audio features  
extreme loudness values  
incomplete metadata

This stage ensures that all candidates contain
the required Spotify audio features.

---

# 6 Signal Mapping

Spotify audio features are converted into MPM signals.

Spotify features:

danceability  
energy  
tempo  
loudness  
valence  
speechiness  
acousticness  
instrumentalness  

These features are mapped to:

melody_signal  
rhythm_signal  
timbre_signal  
arrangement_signal  

The mapping process is defined in:

07_MPM_signal_mapping.md

---

# 7 MPM Scoring

Each candidate track is evaluated by the MPM inference engine.

Inputs:

listener_personality  
mpm_signals

MPM outputs:

predicted_score  
confidence  
recommend_score

These values represent the predicted listening experience.

---

# 8 Preference Filter

Tracks may be filtered before inference if severe preference conflicts occur.

Example filters:

if intro_penalty ≥ 2.0
→ reject track

if timbre_penalty = 1
→ reduce recommendation priority

---

# 9 Recommendation Safety Filter

MPM uses a recommendation safety filter
to avoid risky recommendations.

Example thresholds:

predicted_score ≥ 6.5  
confidence ≥ 0.80  
recommend_score ≥ 0.80  

Tracks failing these thresholds are removed.

This step prioritizes **safe recommendations**
over recommendation volume.

---

# 10 Ranking

Remaining tracks are ranked by:

recommend_score

Higher recommend scores indicate stronger
recommendation safety and reward potential.

The system selects the top-ranked tracks.

Typical recommendation list size:

3–5 tracks.

---

# 11 Recommendation Output

The final recommendation output includes:

artist  
song  

predicted_score  
confidence  

reason explanation

Spotify link

The output format is defined in:

10_MPM_output_spec.md

---

# 12 Design Principles

The MPM discovery system follows several principles.

Separation of discovery and scoring

Candidate discovery generates possible tracks,
while MPM scoring evaluates listener compatibility.

Safety-first recommendation

The system prioritizes avoiding bad recommendations.

Structure-based scoring

MPM evaluates structural reward compatibility
rather than genre similarity.

---

# 13 Summary

The discovery pipeline transforms the full music universe
into a small set of high-quality recommendations.

Pipeline overview:

Spotify API  
↓  
Candidate Generation  
↓  
Signal Mapping  
↓  
MPM Inference  
↓  
Recommendation Filter  
↓  
Final Recommendations
