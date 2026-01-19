# Spotify Vibe Shift: A/B Test Analysis

**Ridham Patel** | Data Science and Applied Statistics Student | Purdue University | January 2026

A portfolio project analyzing whether recommending opposite music to users increases engagement and discovery on a music streaming platform.

**Data Source:** [Spotify Dataset on Kaggle](https://www.kaggle.com/datasets/joebeachcapital/30000-spotify-songs)

---

## Quick Note About This Repo

I built this entire project locally on my computer first (working through the analysis over winter break), so that's why there's one big commit uploading everything at once. I know it's not the best git practice - normally you'd want to commit as you go - but I didn't think to initialize a GitHub repo until I finished and wanted to add it to my portfolio. 

---

## What This Project Is About

For one of my portfolio projects, I wanted to explore A/B testing and experimental design. I came up with the idea of testing a Vibe Shift feature for a music streaming platform that recommends songs that are emotionally opposite to what you normally listen to.

The hypothesis: pushing users outside their comfort zones would increase music discovery and engagement.

---

## The Question I Wanted to Answer

Does recommending music that's opposite to users' current taste increase engagement?

And more specifically - does it work the same for all users, or are there differences based on how diverse their music taste already is?

---

## What I Did

### 1. Data Cleaning & Preparation
Started with a Spotify dataset from Kaggle (~32k songs across different playlists). Cleaned it up, removed duplicates, handled missing values.

### 2. Exploratory Data Analysis
- Analyzed audio features (valence, energy, danceability, etc.)
- Decided to focus on just valence (happy/sad) and energy (energetic/calm) since they're the most emotionally meaningful
- Created comfort zone scores based on how diverse each user's listening history is
- Used K-means clustering to segment users into three groups: Narrow, Moderate, and Wide comfort zones

### 3. Experiment Design & Simulation
Since I don't have access to actual user data or the ability to run a real A/B test, I simulated one:
- Randomly assigned users to Control (normal recommendations) vs Treatment (opposite recommendations)
- Used stratified sampling to balance segments across both groups
- Simulated realistic engagement patterns based on user segments
- Created guardrail metrics to catch potential violations

### 4. Statistical Analysis
- T-tests to check significance
- Cohen's d for effect sizes 
- Confidence intervals
- Segmented analysis to see heterogeneous treatment effects

---

## Key Findings

**TL;DR:** The feature works really well for some users, not at all for others.

### Overall Result
+9.7% engagement increase (p<0.001) - highly significant!

But when you break it down by user segment, the story gets more interesting:

### By User Segment

**Moderate Comfort Zone Users: Winner**
- +11.9% engagement (p<0.001)
- Medium effect size (Cohen's d = 0.52)
- Guardrails passed (skip rate acceptable)
- **Recommendation: Launch for this segment**

**Narrow Comfort Zone Users: Problem**
- +10.1% engagement (barely significant, p=0.03)
- BUT skip rate violated guardrail (+11.9pp increase)
- Users are skipping almost half their recommendations
- **Recommendation: Launch strategically**

**Wide Comfort Zone Users: No Effect**
- +6.0% lift but not significant (p=0.13)
- Makes sense - they're already exploring
- **Recommendation: Don't launch - no benefit**

### The Surprising Part

I honestly thought users with the narrowest taste (Narrow segment) would benefit most since they're the most stuck. But the data showed they need a gentler push, not maximum disruption. The Moderate users are the sweet spot.

This is what we learned about in my ML class as heterogeneous treatment effects - same intervention, different outcomes for different groups.

---

## What I'd Actually Recommend

After running the analysis, here's what I think should happen:

**Launch for Moderate users with full-opposite recommendations.** They showed +11.9% engagement (p<0.001), medium effect size, and all guardrails passed. They're the sweet spot - stuck enough to benefit from disruption but not so stuck that it feels jarring.

**Don't launch for Narrow users yet - but don't give up on them either.** The full-opposite approach failed the skip rate guardrail (+11.9pp increase), but they showed the highest discovery increase at +12.9%. So they are willing to explore, we're just pushing too hard. I'd test three adapted approaches:
- **Gradual shift:** Ease them in with 25% → 40% → 60% → 80% over multiple weeks
- **Genre constraint:** Keep genre familiar but shift emotions (high-energy rap → sad/calm rap)

- **User toggle:** Adventure Mode that they control for both Moderate and Narrow users in case they want to switch back to their old patterns.

My bet is gradual shift works, but needs testing.

**Skip Wide users entirely.** No significant effect (p=0.13) - they're already exploring on their own.

This is a example of heterogeneous treatment effects from my ML course - same feature, totally different outcomes depending on who uses it.

**[Full Business Recommendations & Analysis →](Business_Recommendations.md)**

---

## Technical Details

**Tools Used:**
- Python (pandas, numpy, scipy, matplotlib, seaborn, sklearn)
- Jupyter Notebooks
- Statistical testing (t-tests, effect sizes, confidence intervals)
- K-means clustering for segmentation

**Methods:**
- Stratified random assignment for A/B test
- Two-sample t-tests for significance
- Cohen's d for effect size measurement
- Guardrail metrics to prevent shipping bad UX

**Dataset:**
- Source: Kaggle Spotify dataset
- ~32k songs after cleaning
- 470 playlists treated as unique users
- Features: valence, energy, acousticness, etc.

---

## Repository Structure

```
├── notebooks/
│   ├── 1_Data_Cleaning.ipynb          # ETL and data prep
│   ├── 2_EDA.ipynb                    # Exploration and segmentation
│   ├── 3_Experiment.ipynb             # A/B test simulation
│   └── 4_Stat_Test.ipynb              # Statistical analysis
├── data/
│   └── (data files - not included, download from Kaggle)
├── Business_Recommendations.md         # Main deliverable
└── README.md                          # You are here
```

**Note:** I didn't include the data files since they're large and from Kaggle. If you want to run this yourself, download the Spotify dataset from Kaggle and put it in the data folder.

---

## What I Learned

### Technical Skills
- How to design and simulate an A/B test properly
- Stratified sampling and why it matters
- Effect sizes vs statistical significance (both matter!)
- Why guardrail metrics are crucial
- K-means clustering for user segmentation

### Analytical Thinking
- One size doesn't fit all - segment your users
- Your hypothesis can be wrong 
- Stuck users ≠ ready to change users
- Discovery alone doesn't guarantee engagement
- Guardrails prevent you from shipping something that technically works but users hate

### Things I'd Do Differently
- Start the GitHub repo from the beginning instead of one big commit at the end
- Would love to run this on actual user data if I get access to it
- Could explore more sophisticated segmentation methods
- Maybe test different shift intensities instead of just full opposite

---

## Running the Notebooks

If you want to run these yourself:

1. **Get the data:**
   - Download Spotify dataset from Kaggle
   - Place in `data/` folder

2. **Install dependencies:**
   ```bash
   pip install pandas numpy scipy matplotlib seaborn scikit-learn jupyter
   ```

3. **Run notebooks in order:**
   - Start with `1_Data_Cleaning.ipynb`
   - Then `2_EDA.ipynb`
   - Then `3_Experiment.ipynb`
   - Finally `4_Stat_Test.ipynb`

4. **Check the recommendations:**
   - Read `Business_Recommendations.md` for the full analysis and recommendations

---

## Limitations & Context

**Important:** This is a portfolio project using simulated treatment effects. I created realistic data to demonstrate experimental design and analysis skills, but in a real setting you'd:
- Run an actual A/B test with real users
- Track long-term effects (retention, lifetime value)
- Have product/UX researchers interview users
- Test in pilot markets first

Also, I made some simplifying assumptions (like treating each playlist as one user) that wouldn't hold in production but work fine for demonstration purposes.

---

## Future Work / Ideas

If I had more time or was actually implementing this:

- **Test "softer" versions for Narrow users** - Instead of full opposite recommendations, try a 30-40% shift
- **Personalized shift intensity** - Adapt the opposite-ness based on user response over time
- **Long-term retention analysis** - Does the engagement boost actually lead to better retention?
- **Cross-platform behavior** - Do people respond differently on mobile vs desktop?
- **Add user research** - Interview users about their experience, not just look at metrics

---

## Why I Built This

I wanted to demonstrate end-to-end A/B test analysis for my portfolio, but I also genuinely think this is an interesting product idea. Music discovery is hard - people get stuck in what they know. But forcing random recommendations doesn't work either (see: Narrow users violating skip rate).

The targeted approach (launch for the right users only) is more complex but shows you're thinking about product-market fit even within your existing user base. That's the kind of nuanced thinking I'm trying to develop as I learn more about data science and product analytics.

---

## Coursework Connection

This project applies concepts from:
- **STAT 355:** Hypothesis testing, t-tests, confidence intervals
- **CS 373:** Machine Learning (K-means, effect sizes, experimental design)
- **Data Science Projects course:** End-to-end analysis workflow

It's cool to see how stuff from different classes comes together in a real(ish) project.

---

## Contact

If you have questions about the analysis or want to discuss the project, feel free to reach out!

**Email:** pate2017@purdue.edu

**LinkedIn:** https://www.linkedin.com/in/ridhampatel13/

---

**Last Updated:** January 2026

*Built this over winter break as a portfolio project. Hope it's useful to see how A/B testing and experimental design work in practice!*
