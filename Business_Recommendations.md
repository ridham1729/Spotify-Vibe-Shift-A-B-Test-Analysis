# Business Recommendations: Spotify Vibe Shift A/B Test

**Ridham Patel** | January 2026

---

## Summary

Launch for Moderate users. Test a gentler version for Narrow users. Skip Wide users.

Thought Narrow users (most stuck in their bubble) would love being pushed to explore, but full-opposite recs were way too jarring. Plot twist though - they showed the highest discovery increase (+12.9%), so there's definitely something there if we fix the execution.

---

## Quick Background

### The Feature Idea

Vibe Shift recommends music that's emotionally opposite to what you normally listen to. If you're always playing upbeat Taylor Swift, it recommends slower, sadder Billie Eilish. Based on two dimensions: valence (happy/sad) and energy (energetic/calm).

Goal: push people outside their comfort zones to discover new music.

### How I Segmented Users

Divided everyone into three groups based on how diverse their listening history is (measured by comfort zone score = avg std dev of valence and energy):

**Narrow users** (score <0.150): Super consistent taste, don't explore much on their own. Baseline ~5 songs liked per session.

**Moderate users** (score 0.150-0.184): Some variety but still have clear preferences. Baseline ~7 songs per session.

**Wide users** (score >0.184): Already listen to all kinds of stuff. Baseline ~9 songs per session.

My hypothesis going in: Narrow users would benefit most since they're the most stuck.

---

## What the Numbers Showed

### Overall Effect: +9.7% Engagement

Pooling everyone together, treatment increased engagement by 9.7% (p=0.0004 - way below the 0.05 threshold from stats class). Users liked about 0.67 more songs per session, roughly 1 extra song every 2 sessions.

For a music platform, that's actually pretty substantial. More liked songs usually correlates with retention.

But that 9.7% average completely masks what's happening at the segment level.

### Breaking Down by Segment

**Moderate users: +11.9% lift** (p<0.001, Cohen's d=0.52)
- Medium effect size, highly significant
- Skip rate: +2.8pp (went up but under my 10pp threshold)
- Verdict: Launch it

**Narrow users: +10.1% lift** (p=0.030, Cohen's d=0.37)  
- Barely significant, small effect size
- Skip rate: +11.9pp (violates my 10pp threshold - users skipping 47% of recs!)
- Verdict: Don't launch THIS version, but don't give up

**Wide users: +6.0% lift** (p=0.13, Cohen's d=0.29)
- Not significant - could be random noise
- Skip rate: basically unchanged
- Verdict: Skip this segment, no value

### Discovery Rates (The Interesting Part)

ALL segments explored more diverse music:
- Narrow: +12.9% more diversity (highest!)
- Moderate: +8.6%
- Wide: +5.3%

So the algorithm mechanically works - it's pushing people to explore. Question is whether that discovery feels good or frustrating. For Moderate users: yes. For Narrow users: discovery's happening but UX is bad.

---

## Why Each Segment Responded Differently

### Moderate Users: The Sweet Spot

This is where everything clicked. +11.9% engagement with p<0.001 (extremely confident it's real), medium effect size, and guardrails all passed.

These users are stuck enough that opposite recommendations help them discover stuff they wouldn't find on their own, but not so stuck that it feels wrong. They skip a few extra songs while exploring (+2.8pp increase) but ultimately engage more. Perfect product-market fit.


### Narrow Users: High Potential But We're Doing It Wrong

This segment surprised me the most. I really thought they'd show the biggest effect since they're literally the most stuck in their music bubble.

What happened: engagement did technically go up (+10.1%, p=0.030), but skip rate jumped from 35% to 47%. They're skipping almost HALF their recommendations. That violates my guardrail - I set the threshold at max 10pp increase.

Here's what's interesting though: they showed the HIGHEST discovery increase at +12.9%. This tells me they ARE willing to explore when they find something they like. We're just pushing too hard.

Think about it - if you only ever listen to high-energy rap, getting recommended slow acoustic folk is jarring as hell. You'll skip through tons of songs trying to find something tolerable. But that +12.9% discovery means when they DO engage, they're exploring more than anyone else.

Key insight I had: being stuck doesn't mean not ready to change. It means they need a bridge, not a teleport. Full opposite (going from 0.8 energy to 0.2 energy) is too big a jump.

So instead of writing off this segment, I think we should test adapted approaches. They have too much potential to ignore.

### Wide Users: Nothing Happening

+6.0% lift but not significant (p=0.13). Confidence interval includes zero, so could just be noise.

Makes total sense. These users already explore diverse music on their own. Opposite recommendations don't add value - they probably would've discovered that music anyway.

Skip this segment.

---

## What I'd Actually Recommend

### For Moderate Users: Launch Full-Opposite

Straightforward. The feature works as-is.

Calculate comfort zone scores from listening history (avg std dev of valence/energy). Moderate users fall in the 0.150-0.184 range. Use full opposite algorithm (target = 1 - current value).

Start with 10% pilot, monitor engagement/skip rate/session length daily, scale to 100% if metrics hold.

### For Narrow Users: Test These Adaptations

That +12.9% discovery is too good to give up on. The problem isn't that they don't want to explore - it's that we're executing it wrong. Here are three approaches I'd test, in order:

**First: Gradual shift**

Instead of jumping straight to full opposite, ease them in over time. Week 1: shift only 25% toward opposite. Week 2: if they engaged with week 1 stuff, go to 40%. Week 3: 60%. Week 4+: cap at 80% max (never full opposite for this group).

Why I think this works: reduces the initial shock. Going from high-energy rap to slightly less energetic rap feels way less jarring than jumping to acoustic folk. They can adjust expectations gradually and build trust.

Also easiest to implement - just one parameter adjustment, no new constraints or UI needed.

Test it: small A/B with Narrow users only. Success = skip rate stays under 10pp and engagement lifts 5%+. If it works, scale. If not, try option 2.

**Second: Keep genre similar**

Recommend opposite emotions (happy→sad, energetic→calm) but constrain to same genre. So high-energy rap → sad/calm rap, not slow folk.

Why: maintains familiarity while still pushing emotional exploration. The sound stays similar even if the vibe changes.

### For Wide Users: Skip It

Not significant, no value. Don't waste engineering resources here.

### For Moderate and Narrow users both - 

Add a UI toggle and only give opposite recs when they turn it ON. Default should be OFF. Don't call it opposite recommendations - that sounds disruptive. Frame it positively like Expand Your Taste or Discovery Mode.

Why: user agency. When they choose to explore, they're mentally prepared for something different. Skip rate should be lower.
---

## Why Guardrails Mattered

I set two guardrails before running the test:

**Skip rate:** can't increase more than 10pp  
**Session length:** can't drop more than 5%

All segments passed session length (people aren't quitting early). But Narrow users failed skip rate - jumped 11.9pp, well over threshold.

This is exactly why guardrails matter. Without that threshold, I might have just looked at the +10.1% engagement lift and said launch it! But users would've been frustrated even though the metric technically improved.

The violation doesn't mean give up on this segment though - it means launch strategically. That's why I'm focusing on the adapted approaches.

---

## What I Learned

**Being stuck ≠ not ready to explore**

Biggest takeaway. Narrow users showed the highest discovery increase (+12.9%), which means they are willing to explore. They just don't want to be thrown in the deep end. 

**Heterogeneous treatment effects are real**

This is what I've read about Heterogeneous treatment effects - same intervention, completely different outcomes by segment. The +9.7% overall masks that it works great for some users and needs major fixes for others.

**Guardrails prevent shipping bad UX**

Without the skip rate threshold, I would've launched full-opposite for everyone based on overall lift. Would've frustrated a whole segment while technically winning on the primary metric.

**Creative problem-solving > binary yes/no**

Instead of just launch or don't, this project taught me to think about adaptations. Different segments might need different implementations. 

---

## Limitations & Context

This is a portfolio project using simulated data. The treatment effects are based on my assumptions about how users would respond.

In production, I'd need to:
- Run actual A/B test with real users (minimum 2 weeks)
- Get user feedback, especially from Narrow users about gradual shift
- Track long-term effects (does the boost stick or fade after novelty wears off?)
- Way bigger sample size (I only had 120 Narrow users)
- Test across platforms (mobile vs desktop might differ)

But for demonstrating experimental design, segmentation, and product thinking, simulated data works fine. The analysis approach is what matters for a portfolio piece.

---

## Final Thoughts

Launch for Moderate users - they're ready now.

Test adapted versions for Narrow users - that +12.9% discovery potential is too good to ignore. Try gradual shift first, genre constraint if that doesn't work, toggle as backup.

Skip Wide users - no benefit there.
---

**Methods:** Two-sample t-tests, Cohen's d, 95% CIs, α=0.05 (standard stats stuff from coursework)
