# Listening Style Questionnaire (LSQ)

Version: v8  
Layer: Personality Layer

---

# 1 Overview

The Listening Style Questionnaire (LSQ) is used to estimate a listener's
music perception profile.

The questionnaire measures how strongly a listener naturally attends to
different musical structures during listening.

The LSQ does not measure genre preference or stylistic taste.

Instead, it estimates the listener’s sensitivity to core musical
structures that influence reward generation.

The questionnaire currently measures four perception dimensions:

melody  
rhythm  
timbre  
arrangement

These dimensions correspond to the primary perceptual axes used by the
Music Personality Model (MPM).

The LSQ provides a **cold-start estimate** of listener perception weights
that are later used by the MPM inference system.

---

# 2 Response Scale

Each question should be answered using a **1–7 Likert scale**.

1 = strongly disagree  
2 = disagree  
3 = slightly disagree  
4 = neutral  
5 = slightly agree  
6 = agree  
7 = strongly agree

Respondents should answer based on their **natural listening behavior**.

There are no correct or incorrect answers.

---

# 3 Melody Perception

Melody perception measures how strongly the listener focuses on
melodic structure and pitch movement.

Listeners with high melody sensitivity often respond strongly to:

• melodic development  
• melodic resolution  
• motif repetition  
• melodic hooks

Questions:

Q1  
I often notice and remember melodies when listening to music.

Q2  
Melodic variation quickly attracts my attention.

Q3  
I can easily follow complex melodic lines in music.

---

# 4 Rhythm Perception

Rhythm perception measures sensitivity to temporal structure,
groove, and rhythmic propulsion.

Listeners with high rhythm sensitivity often respond strongly to:

• beat clarity  
• groove continuity  
• rhythmic momentum  
• tempo-driven energy

Questions:

Q4  
Strong rhythm naturally captures my attention.

Q5  
I quickly notice rhythm changes in music.

Q6  
Music without a clear groove quickly loses my interest.

---

# 5 Arrangement Perception

Arrangement perception measures sensitivity to structural organization
within a track.

Listeners with high arrangement sensitivity often notice:

• structural transitions  
• compositional structure  
• phrase development  
• musical form

Questions:

Q7  
I often notice how a song is structured or arranged.

Q8  
I pay attention to how different sections of a song connect.

Q9  
Complex musical structure makes a track more interesting to me.

---

# 6 Timbre Perception

Timbre perception measures sensitivity to sound texture and sonic
quality.

Listeners with high timbre sensitivity often respond strongly to:

• sound texture  
• production quality  
• layering and spatial depth  
• instrumental tone

Questions:

Q10  
Sound texture strongly affects how much I enjoy music.

Q11  
Production quality is very important in my evaluation of a song.

Q12  
Layering and sonic atmosphere make music more immersive.

---

# 7 Questionnaire Summary

The LSQ consists of **12 questions** divided across four perception
dimensions:

melody      → Q1–Q3  
rhythm      → Q4–Q6  
arrangement → Q7–Q9  
timbre      → Q10–Q12

Responses are later converted into **perception weights** used by the
MPM personality generator.

The LSQ is intentionally kept compact to reduce friction during
cold-start initialization.

Engineering refinements in the inference layer should not automatically
expand the questionnaire dimensions.

---

# 8 Role in MPM Pipeline

The LSQ is the entry point of the **personality generation pipeline**.

Pipeline structure:

LSQ questionnaire  
↓  
LSQ scoring  
↓  
personality generation  
↓  
MPM inference

The questionnaire itself only collects responses.

Score computation and personality generation are defined in
separate documents.
