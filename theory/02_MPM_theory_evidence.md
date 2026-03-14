# MPM Dataset Summary

Version: v8  
Status: Research Prototype

---

# 1 Overview

The Music Personality Model is not purely theoretical.

The conceptual framework was derived from repeated listening observations
collected during the development of the prototype system.

These observations were recorded in a structured dataset
that captures listener reactions to music across multiple perceptual dimensions.

The MPM dataset supports research on **structure-based, perception-driven music recommendation**.

Unlike traditional recommendation systems that rely primarily on behavioral similarity,
MPM focuses on modeling how listeners extract reward from musical structure.

The dataset is therefore designed to capture:

• listener feedback  
• perceptual reactions  
• reward response patterns  
• early failure mechanisms  
• recommendation failure cases  

The core interpretation of the dataset is:

genre-free  
style-free  
structure-based  
reward-based  

The goal is not to learn which genre a listener likes.

The goal is to understand which **structural reward patterns**
produce positive, neutral, or negative outcomes for a listener.

---

# 2 Dataset Structure

Dataset Size

The current prototype is calibrated using a manually labeled dataset
of over 500 music samples evaluated by the primary listener (user0).

Each sample contains a 1–7 reward rating derived from real listening behavior.

Earlier exploratory experiments included additional samples,
bringing the total number of evaluated tracks to approximately 600+.

This dataset was used to calibrate the reward model
and listener personality parameters for the prototype.

Ratings were collected through repeated listening sessions
to ensure stable reward evaluation.

Each listening sample may contain the following information:

id  
artist  
song  
duration
skip_point
overall_rating (1=very bad, 7=very good)
melody_rating (1=very bad, 7=very good)
tempo rating (1=very bad, 7=very good)
timbre rating (1=very bad, 7=very good)
Arrangement Rating (1 = very bad, 7 = very good) 
Perceived BPM (1 = Very Slow, 7 = Very Fast)
skipcat
Skip Reason Notes
Familiarity (1 = Very Unfamiliar, 7 = Very Familiar)
Willingness to Replay (1 = Very Unwilling, 7 = Very Willing)
real BPM
  

Example:

id: 422  
artist: Coldplay 
song: The Scientist
duration: 5:09
skip_point: 1:22
overall_rating (1=very bad, 7=very good): 2
melody_rating (1=very bad, 7=very good): 3
tempo rating (1=very bad, 7=very good): 2
timbre rating (1=very bad, 7=very good): 3
Arrangement Rating (1 = very bad, 7 = very good):4
Perceived BPM (1 = Very Slow, 7 = Very Fast): 2
skipcat: 2
Skip Reason Notes: no rhythm
Familiarity (1 = Very Unfamiliar, 7 = Very Familiar): 2
Willingness to Replay (1 = Very Unwilling, 7 = Very Willing): 1
real BPM: 75

---

# 3 Rating Scale

Listener ratings follow a **1–7 Likert scale**.

1 — Very bad  
2 — Bad  
3 — Slightly bad  
4 — Neutral  
5 — Fairly good  
6 — Good  
7 — Very good  

The rating reflects **overall perceived reward** from the track.

---

# 4 Skip Category (skipcat)

The skip category records **why a listener stopped listening**.

1 = melody caused skip  
2 = rhythm caused skip  
3 = timbre caused skip  
4 = arrangement caused skip  
5 = emotional response caused skip  
6 = song completed  

Skip categories help identify which perceptual dimension or negative mechanism triggered disengagement.

---

# 5 Listener-Level Structural Dimensions

At the personality level, MPM currently uses four main perceptual dimensions:

melody  
rhythm  
timbre  
arrangement  

These remain the core listener-level axes used for:

• LSQ mapping  
• perception weights  
• personality generation

The dataset continues to organize listener personality around these four dimensions.

---

# 6 Observed Listener Reactions

During listening tests, several common reaction patterns were observed:

• attention loss during long static intros  
• discomfort caused by harsh timbre  
• disengagement during reward drought  
• confusion caused by unpredictable structure  
• neutral compatibility despite structural fit  
• low-propulsion but high-reward outcomes through phrase structure  
• early reward failure despite apparent compatibility

These patterns correspond to mechanisms in the MPM cognitive model.

---

# 7 Dataset Purpose

The MPM dataset supports research into:

• structure-based music evaluation  
• attention dynamics during listening  
• prediction-based reward modeling  
• recommendation failure analysis  
• neutral compatibility detection  
• low-propulsion / high-reward structural patterns  
• early recommendation safety mechanisms

The dataset is primarily used to validate the Music Personality Model (MPM).

---

# 8 Current Limitations

The current dataset is limited in size and listener diversity.

Most samples come from a small number of listeners.

Most samples currently come from listener user0.

The dataset is still exploratory and does not yet support strong statistical generalization.

Future work may expand the dataset to include:

• more listener personalities  
• broader structural coverage  
• longer listening sessions  
• more phrase-level annotations  
• more fine-grained rhythm annotations  

---

# 9 MPM vs Spotify Blind Listening Experiment

## 1 Experiment Objective

The purpose of this experiment was to evaluate whether the Music Personality Model (MPM) can produce music recommendations that better match listener preference compared to a widely used commercial recommender system.

The comparison target was Spotify's recommendation system, which primarily relies on collaborative filtering and listening behavior similarity.

The experiment evaluates whether modeling listener perception and reward mechanisms provides measurable advantages.


---

## 2 Experimental Setup
Listener
user0

Creator of the Music Personality Model.

All ratings were performed by the same listener to ensure internal consistency.

## Rating Scale

Songs were evaluated using a 1–7 Likert scale:

Score	Meaning
1	very bad
2	bad
3	somewhat bad
4	neutral
5	somewhat good
6	good
7	very good

## Method

Step 1

Select 10 seed songs that match the listener's preference.
The seed songs were selected to match the listener’s preference profile to ensure a fair comparison.


Step 2

Spotify generates a recommendation playlist(20 songs).


Step 3

MPM generates a prediction playlist(20 songs) using the structural perception model.


Step 4

Songs are evaluated through blind listening.
The listener did not know which system generated each recommendation during evaluation.

## Seed Songs

The Spotify recommendation system was seeded using the following songs:

Static-X — Otsegolectric
Universum — Faded
Scar Symmetry — Reborn
Sonic Syndicate — Jack of Diamonds
The Unguided — Phoenix Down (Zardonic Remix)
Alien Vampires — No Way Back (CyGnosic Mix)
Feint — Snake Eyes
Lemon Fight — Stronger
Rammstein — Feuer Frei
Sybreed — Doomsday Party

Spotify generated recommendations based on these seeds.

## Recommendation Samples

Two recommendation sets were evaluated:

Spotify recommendations: 20 songs
MPM recommendations: 20 songs

All songs were evaluated under blind listening conditions.

The listener did not know which system produced each recommendation.

## 4 Result

### Dataset

The blind listening experiment used the dataset:

test_01_mark.xlsx

Songs were labeled using a source column:

1 = MPM recommendation
2 = Spotify recommendation

Sample size:

System	Songs
MPM	20
Spotify	20

Total evaluated songs:

40

Ratings were recorded on a 1–7 Likert scale.

## Rating Distribution

The distribution of listener ratings for both recommendation systems is shown below.

Rating	MPM	Spotify
1	    0	    0
2	    0	    6
3	    1   	4
4	    10	    8
5	    5	    1
6	    2   	1
7	    2	    0

### Interpretation

The distribution shows a clear difference between the two systems.

Spotify recommendations are concentrated in the 2–4 range, indicating frequent mismatch with listener preference.

MPM recommendations are concentrated in the 4–6 range, indicating better compatibility with the listener's perceptual reward response.

Notably:

Spotify produced six rating-2 songs, indicating multiple strongly disliked recommendations.

MPM produced zero songs rated 1 or 2.

This supports the earlier observation that MPM significantly reduces recommendation risk.

The rating distribution shows that MPM shifts recommendations from the low-reward region (2–3) toward the neutral and positive reward region (4–6), while maintaining similar variance compared to Spotify.

## Standard Deviation

The standard deviation of ratings for each system is:

System	Mean	Standard Deviation
MPM	4.70	1.08
Spotify	3.35	1.14

### Interpretation

Both systems exhibit similar variability in listener ratings.

However, the mean rating of MPM is significantly higher, while maintaining comparable variance.

This indicates that the improvement in average rating is not caused by reduced variance but by a genuine shift toward higher listener satisfaction.

## Confidence Interval of Mean Rating

To estimate the reliability of the observed mean ratings, a 95% confidence interval (CI) was calculated for both recommendation systems.

System	Mean	95% CI
MPM	    4.70	[4.19, 5.21]
Spotify	3.35	[2.82, 3.88]

### Interpretation

The confidence intervals show that the true average listener rating for each system is highly likely to fall within the reported ranges.

Notably, the two confidence intervals do not overlap.

This provides additional statistical evidence that the difference between the two recommendation systems is meaningful rather than random variation.

The results therefore support the conclusion that:

MPM recommendations achieve consistently higher listener satisfaction than Spotify recommendations.


### Rating Performance
Mean Rating
System	Mean Rating
MPM     4.70
Spotify	3.35

### Difference:

Δ = +1.35

MPM recommendations achieved a substantially higher average listener rating.

### Median Rating
System	Median
MPM	    4.0
Spotify	3.5

The distribution of MPM recommendations is shifted toward higher ratings.

## Recommendation Quality
### Bad Recommendation Rate

Bad recommendations were defined as:

rating ≤ 3
System	Bad Recommendation Rate
MPM	5%
Spotify	50%

Spotify produced ten times more clearly negative recommendations.

### High-Quality Recommendation Rate

High-quality recommendations were defined as:

rating ≥ 6
System	High-Quality Rate
MPM	20%
Spotify	5%

MPM produced four times more highly rated songs.

## Statistical Significance

A Welch two-sample t-test was conducted on the rating distributions.

Results:

t = 3.8489
p = 0.00044

Interpretation:

p < 0.001

The difference between MPM and Spotify recommendation ratings is statistically significant.

## Effect Size

Effect size was measured using Cohen’s d.

Cohen's d = 1.22

Interpretation:

d	Effect Size
0.2	small
0.5	medium
0.8	large

The observed value:

d ≈ 1.22

indicates a very large effect size, meaning the rating distribution of MPM recommendations is more than one standard deviation higher than Spotify recommendations.

## Combined Statistical Summary

For clarity, the main statistical results of the experiment are summarized below.

Metric	MPM	Spotify
Mean rating	4.70	3.35
Median rating	4.0	3.5
Standard deviation	1.08	1.14
95% CI	[4.19, 5.21]	[2.82, 3.88]
Bad recommendation rate	5%	50%
High-quality recommendation rate	20%	5%
Cohen's d	1.22	—
Welch t-test	p = 0.00044	—

## Key Statistical Insight

The statistical analysis reveals a clear shift in recommendation quality:

MPM shifts the rating distribution upward by more than one Likert point while maintaining similar variance.

Combined with the large effect size and statistically significant test result, the experiment provides strong empirical support for the effectiveness of the Music Personality Model.

## Probability of Superiority

To provide an intuitive interpretation of the effect size, the Common Language Effect Size (CLES) was calculated.

This metric represents the probability that a randomly selected recommendation from one system receives a higher rating than a randomly selected recommendation from another system.

Result:

P(MPM rating > Spotify rating) ≈ 81%

## Interpretation

This result means that:

If one MPM recommendation and one Spotify recommendation are selected at random,
there is approximately an 81% probability that the MPM recommendation will receive a higher listener rating.

This provides an intuitive explanation of the previously reported effect size:

Cohen's d = 1.22

which corresponds to a very large difference between the two rating distributions.

## Relationship to Experimental Results

The probability result is consistent with the earlier statistical findings:

Metric	Result
Mean rating improvement	+1.35
Effect size	d ≈ 1.22
Statistical significance	p = 0.00044
Probability of superiority	≈ 81%

Together, these metrics indicate a strong and consistent advantage for the Music Personality Model in predicting listener satisfaction.

Across all statistical measures, the Music Personality Model demonstrates a substantial and statistically significant advantage over Spotify recommendations in predicting listener satisfaction.

## Recommendation Risk Analysis
### Definition of Recommendation Risk

In recommendation systems, one of the most critical performance indicators is the risk of delivering poor recommendations.

A bad recommendation negatively impacts user experience and reduces trust in the system.

For this analysis, recommendation risk is defined as:

rating ≤ 3

These ratings indicate clear listener dissatisfaction.

### Bad Recommendation Probability
System	Bad Recommendation Rate
MPM	    5%
Spotify	50%

Interpretation:

Spotify produces poor recommendations in half of the cases,
while MPM produces poor recommendations only rarely.

Risk reduction:

MPM reduces bad recommendations by 90%.

### Recommendation Safety

Recommendation safety measures how reliably a system avoids negative experiences.

We define a safe recommendation as:

rating ≥ 4

Results:

System	Safe Recommendation Rate
MPM	85%
Spotify	50%

Interpretation:

MPM recommendations are far more likely to produce neutral or positive listener responses.

## Risk Distribution

The rating distributions show a clear shift in recommendation risk.

Spotify recommendations concentrate heavily in the 2–4 range, indicating frequent mismatches with listener preference.

MPM recommendations cluster around 4–6, indicating better compatibility with listener perception and reward response.

## Practical Implications

In real-world music streaming platforms, recommendation failures have a large impact on user engagement.

Poor recommendations often lead to:

immediate track skipping

session abandonment

reduced trust in the recommendation system

The experimental results suggest that MPM's perceptual compatibility model significantly reduces this risk.

## Risk Advantage of MPM

Key improvements observed:

Bad recommendation rate reduced from 50% → 5%
Recommendation safety increased from 50% → 85%

This indicates that MPM is substantially better at avoiding negative listening experiences.

## Interpretation

The primary advantage of the Music Personality Model is not only higher average ratings but lower recommendation risk.

Traditional recommender systems rely on behavioral similarity:

user history similarity → recommendation

MPM instead evaluates perceptual compatibility between listener personality and music structure:

listener personality
+
music structure
→ predicted reward response

This allows the system to avoid recommending songs that would trigger early rejection or perceptual mismatch.

## Summary of Results
Metric	MPM	Spotify
Mean rating	4.70	3.35
Median rating	4.0	3.5
Bad recommendation rate	5%	50%
High-quality recommendation rate	20%	5%
Key Findings

The blind listening experiment demonstrates that MPM recommendations significantly outperform Spotify recommendations in predicting listener satisfaction.

Key improvements observed:

+1.35 average rating improvement
10× reduction in poor recommendations
4× increase in high-quality songs
large effect size (Cohen’s d ≈ 1.22)
statistically significant difference (p < 0.001)

These results provide strong empirical evidence supporting the effectiveness of the Music Personality Model approach.

## Conclusion

The experiment demonstrates that the Music Personality Model significantly reduces recommendation risk while improving listener satisfaction.

Key findings:

90% reduction in bad recommendations
large effect size (Cohen's d ≈ 1.22)
statistically significant improvement (p < 0.001)

These results suggest that perceptual modeling may provide a promising direction for next-generation music recommendation systems.

## Spotify Failure Pattern Analysis
### Overview

To better understand the differences between the Music Personality Model (MPM) and Spotify recommendations, the songs that received low listener ratings were analyzed.

Low-rating songs are defined as:

rating ≤ 3

This analysis focuses on identifying structural patterns that caused recommendation failures.

The goal is to determine whether these failures can be explained by the perceptual reward model used in MPM.

## Failure Pattern Categories

Analysis of the Spotify recommendation set reveals several recurring failure patterns.

Three major categories were identified.

## 1 Genre Similarity Illusion
### Description

Spotify often recommends songs that are stylistically or culturally similar to the seed songs.

However, similarity in genre or style does not guarantee perceptual compatibility with the listener.

Examples from the Spotify recommendation list include songs that share surface-level genre characteristics with the seed tracks but differ significantly in musical structure.

### Mechanism

Behavior-based recommender systems typically operate using the following logic:

similar users
+
similar listening history
→ recommendation

This approach prioritizes behavioral similarity rather than perceptual compatibility.

As a result, songs that belong to the same genre cluster may still produce very different listener reward responses.

### Impact

Genre similarity without structural compatibility frequently results in ratings around:

2–4

These songs are often listenable but not rewarding, which explains their concentration in the lower half of the rating distribution.

## 2 Structural Chaos Mismatch
### Description

Some recommended songs contain structural patterns that disrupt listener expectations.

These include:

abrupt motif changes

inconsistent rhythmic structure

unstable musical progression

These features can produce structural chaos, which disrupts reward accumulation.

### Mechanism

MPM models this phenomenon using the variable:

structure_chaos

When structural chaos increases, reward accumulation becomes unstable.

In the earlier versions of the model this was handled as a penalty.

Later versions of MPM instead treat chaos as a reward suppression factor, scaling the total reward activation.

### Impact

Songs with high structural chaos often show:

high local energy
low global satisfaction

This results in moderate or low final ratings despite initial engagement.

## 3 Early Timbre Rejection
### Description

Some songs contain harsh or unpleasant sonic characteristics early in the track.

These may include:

aggressive sound textures

distorted production

harsh vocal timbre

Such features can trigger immediate listener rejection.

### Mechanism

MPM models this behavior using early listening gates.

Example gate:

intro_sound_gate

If the sonic characteristics of the intro are perceived as unpleasant, the listener may skip the track before later musical rewards occur.

### Impact

This failure pattern is particularly common in systems that evaluate songs globally rather than temporally.

If later sections of the song are strong, a global model may still recommend the track despite the listener rejecting it early.

## Relationship to MPM Design

The failure patterns identified above correspond directly to mechanisms implemented in the Music Personality Model.

Failure Pattern	MPM Mechanism
Genre similarity illusion	perceptual compatibility model
Structural chaos mismatch	chaos scaling / collapse detection
Early timbre rejection	intro sound gate

These mechanisms were specifically introduced to address limitations observed in behavior-based recommender systems.

## Implications

The analysis suggests that the differences between MPM and Spotify recommendations are not random.

Instead, the observed improvements are consistent with the theoretical design of the MPM architecture.

By modeling the perceptual and reward dynamics of music listening, MPM is able to avoid several common failure modes of behavior-based recommendation systems.

## Conclusion

The failure pattern analysis indicates that many unsuccessful Spotify recommendations can be explained by structural mismatches between the music and the listener's perceptual preferences.

The Music Personality Model addresses these issues by explicitly modeling:

listener perception

predictive expectation

reward accumulation

early rejection mechanisms

This design helps reduce recommendation risk and improve overall listener satisfaction.

