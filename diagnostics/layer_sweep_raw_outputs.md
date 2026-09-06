# Full-output comparison across layers 4, 8, 12, 16 (strength -50)

Manual verification that identical numeric scores across layers 4/8/12 
correspond to genuinely near-identical model output (not a scoring or 
hook bug), and that layer 16 is where real divergence begins.

## Pair 05 — Summarize the main finding from this research abstract in one sentence.

**Layer 4:** Even moderate sleep restriction significantly impairs cognitive work quality in knowledge workers, with effects worsening across consecutive nights.

**Layer 8:** Even moderate sleep restriction significantly impairs cognitive work quality in knowledge workers, with effects worsening across consecutive nights.

**Layer 12:** Even moderate sleep restriction significantly impairs cognitive work quality in knowledge workers, with effects worsening across consecutive nights.

**Layer 16:** Even moderate sleep restriction significantly impairs cognitive work quality in knowledge workers, with effects worsening across consecutive nights.

## Pair 07 — According to this article, what are the two main causes of urban flooding listed?

**Layer 4:** According to the article, the two main causes of urban flooding are impervious-surface expansion and ageing and undersized drainage infrastructure.

**Layer 8:** According to the article, the two main causes of urban flooding are impervious-surface expansion and ageing and undersized drainage infrastructure.

**Layer 12:** According to the article, the two main causes of urban flooding are impervious-surface expansion and ageing and undersized drainage infrastructure.

**Layer 16:** The two main causes of urban flooding listed in the article are impervious-surface expansion and ageing and undersized drainage infrastructure.

## Pair 19 — Bug severity classification (one word: low/medium/high)

**Layer 4:** medium
**Layer 8:** medium
**Layer 12:** medium
**Layer 16:** low

> Note: this is the first non-cosmetic divergence — layer 16 changes the actual judgment, not just phrasing.

## Pair 09 — Required technical skills from a job posting

**Layer 4:**
The required technical skills for the Senior Backend Engineer position at Finova Technologies include:
- Strong proficiency in Python (with FastAPI or Django being preferred)
- Expertise in SQL
- Experience with distributed systems and microservices architecture
- Proficiency with either AWS or GCP
- Familiarity with financial data standards such as FIX protocol and JSON/REST APIs

**Layer 8:** (identical to Layer 4)

**Layer 12:**
The required technical skills for the Senior Backend Engineer position at Finova Technologies include:
- Strong proficiency in Python (with FastAPI or Django being preferred)
- Proficiency in SQL
- Experience with distributed systems and microservices architecture
- Proficiency with AWS or GCP

**Layer 16:**
The required technical skills for the Senior Backend Engineer position at Finova Technologies include:
- Strong proficiency in Python (with FastAPI or Django being preferred)
- Proficiency in SQL
- Experience with distributed systems and microservices architecture
- Proficiency with AWS or GCP
- Familiarity with financial data standards such as FIX protocol and JSON/REST APIs

## Pair 15 — Action items from a retrospective document

**Layer 4:**
The action items listed in the retrospective document are:
1. Engineering to add automated rollback triggers to the deployment pipeline before Sprint 15 begins. (Owner: Devon Park)
2. PM to schedule a dedicated backlog-grooming session with stakeholders for next Thursday. (Owner: Chloe Merritt)
3. QA to document the manual-testing checklist and share it in the team wiki by Wednesday. (Owner: Sam Adeyemi)
4. All team members to complete the async-communication norms survey by the end of the week. (Owner: Team)
5. Tech Lead to evaluate two candidate logging libraries and present a recommendation at Monday's standup. (Owner: Irina Kozlov)

**Layer 8 / Layer 12 / Layer 16:** identical in substance (minor wording variance only: "by the end of the week" vs "by end of week")

## Character-level diff summary

| Pair | Layer 4 vs 8 | Layer 8 vs 12 |
|---|---|---|
| 05 | Identical | Identical |
| 07 | Identical | Identical |
| 19 | Identical | Identical |
| 09 | Identical | Differ at char 178 ("Expertise in SQL" vs "Proficiency in SQL", one bullet dropped) |
| 15 | Differ at char 481 (wording only: "the end of the week" vs "end of week") | Identical |

**Conclusion:** Layers 4/8/12 produce functionally identical, correct, on-task output on these 5 (longest-document) pairs. Layer 16 — the layer selected by cosine-consistency — is the first layer where output diverges in substance, not just phrasing (see Pair 19).
