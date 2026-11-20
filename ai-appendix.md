# AI Appendix

## Sections 1 and 2

Sections 1 and 2 were completed without AI.

Name: Lukas Sadil
Date: 11/20/2026

Using AI in Sections 1 or 2 is an integrity violation. Do not edit the statement above; if it
is not true, talk to the instructor before you submit.

## Section 3: Robustness

### Entry: Exercise 1, instrument strength across samples

- Model and version: Claude Opus 5.5 (Anthropic), claude.ai chat
- Date: 2026-11-20

**[Prep]** At the start of the chat I pasted a setup prompt that gave the role and rules. It said I was working on "Section 3 (robustness) of my ECON 5200 midterm replication of Card (1995)" with nearc4 instrumenting educ and lwage as the outcome. Rules included: "Only edit notebooks/02_robustness.ipynb", "NEVER edit notebooks/01_positioning_and_replication.ipynb", "I must be able to explain every line, so keep code simple and readable", "Do NOT write the "What it implies about the original claim" paragraphs for me", "Credit code taken from the course starter (reference/replication_starter_card1995.ipynb) in a short comment", "Don't commit or push", and "Every number, table and figure must be produced by the notebook and saved to output/". It also gave the baseline to reproduce: N = 3,010, controls exper, expersq, black, south, smsa, smsa66, reg661–reg668, 2SLS educ = 0.1315 (SE 0.0550), OLS = 0.0747, first stage 0.320, F ≈ 13.3. The model could not see my repo: no files were uploaded, and its sandbox had no network and no linearmodels or statsmodels. I chose to work in the chat: "i want to work in this chat seciton by section" and "thats fine we will build code together and i can give you back the terminal answers". I then pasted a quick-reference block listing what is already defined in 02 (df, EXOG, CONTROLS, ols_fit, fs_fit, rf_fit, iv_fit, Panel B fits), ending with "Note: CONTROLS is a string, but the starter functions need a list of column names and data=df."

**[Request]** "Exercise 1: instrument strength across samples. Same working style as before (explain, show the code, wait for my OK, one cell at a time)." Purpose: "test Card's first link, relevance. Does nearc4 actually move schooling strongly, everywhere, or only in part of the sample? My baseline first stage is 0.320 with F ≈ 13.3, barely above the rule of thumb of 10." I asked for a list CTRL_LIST, "first_stage(controls, instrument="nearc4", data=df): adapted from the starter (credit it)" returning coef, SE, classical F, robust HC1 F and n, dropping constant controls in subsamples and reporting them, the 2SLS estimate per sample, the samples full, south, black and smsa splits, "Check: the full-sample row must reproduce 0.320 / F ≈ 13.3 / 2SLS 0.1315", and "Save the table to output/ex1_first_stage.csv".

**[Iterate]**
1. Before the quick reference, the model drafted three cells that rebuilt the sample from fetch(MY_PAPER), defined its own first_stage, and saved output/first_stage_by_sample.csv.
2. After the quick reference, it stopped rebuilding df and instead derived CONTROLS_LIST from the CONTROLS string, with an iv() helper printed next to iv_fit to check it matched.
3. After the Exercise 1 prompt, it said it could not see the starter and asked me to paste first_stage and anderson_rubin. It wrote CTRL_LIST out explicitly with an assert against the CONTROLS string.
4. I pasted the starter code and said "there is not need to include imports as they will all already be present from the set up cell". The model removed the imports and adapted first_stage: data=df instead of card, added the SE, dropped controls with one unique value and returned them as "dropped", and added a credit comment. It warned that the starter's own call first_stage(CONTROLS) would fail with the string CONTROLS.
5. The cell for 2SLS per sample and the save to output/ex1_first_stage.csv with my requested columns was not written in this conversation. The notebook does not include that cell.

**[Mechanism Check]** The model explained that with one instrument the first-stage F equals the t statistic on nearc4 squared, so the robust F is the HC1 t squared. It also explained that a split variable such as south is constant inside its own subsample and perfectly collinear with the constant, and that the assert proves CTRL_LIST and CONTROLS hold the same variables in the same order. I did not ask separate check questions in this conversation.

**[Evaluate]** Checks built into the code: the CTRL_LIST assert; the full-sample first_stage print, expected +0.320, F ≈ 13.3, n = 3,010, dropped []; the earlier helper-vs-iv_fit print, expected 0.1315. I did not paste any output back into the chat, so the model never saw the results. It had offered to cross-check numbers with plain numpy, but no data was uploaded, so no independent check was run. When I ran the cells locally, the assert passed and the full-sample first stage printed +0.320 (SE 0.088), F = 13.3 (robust 14.1), n = 3,010, dropped [], matching my Section 2 baseline.

**Full transcript**

Full transcript: see ai-transcripts/section3_transcript.txt (unedited).

**What I changed, and why:**
- Worked in the chat section by section instead of Claude Code, running code locally and reporting results myself.
- Gave the quick-reference block so the code reuses df, CONTROLS and iv_fit from my notebook instead of rebuilding them.
- Asked for an explicit CTRL_LIST and a starter-based first_stage with credit, so the code follows the course starter and is reused in later exercises.
- Pasted the starter code so the adaptation and credit comment are accurate.
- Removed the imports because the setup cell already has them.
- None. I ran the cells as written, with no edits of my own.

### Entry: Exercise 2, Anderson–Rubin weak-instrument-robust inference

- Model and version: Claude Opus 5.5 (Anthropic), claude.ai chat
- Date: 2026-11-20

**[Prep]** Same setup prompt and quick-reference block as Exercise 1, plus the starter's anderson_rubin code and its markdown notes, which I pasted during Exercise 1.

**[Request]** "Exercise 2: weak-instrument-robust inference (Anderson–Rubin). Same working style." Purpose: "test Card's second link. Can the confidence interval around 0.132 be trusted, given F ≈ 13?" I asked for "anderson_rubin(controls, instrument="nearc4", grid=np.linspace(-0.2, 0.6, 801), data=df, cov_type="HC1"): adapted from the starter (credit it)", to "Run it for the full sample with BOTH cov_type="nonrobust" (apples-to-apples with Card's conventional SEs) and "HC1". If the set touches the grid edge, widen the grid once before calling it unbounded.", then the subsamples, a table saved to output/ex2_anderson_rubin.csv, and a figure saved as output/ex2_ar_intervals.png.

**[Iterate]**
1. The model gave a three-cell plan and wrote Cell 1 only: the adapted anderson_rubin (data=df, cov_type argument, constant-control dropping, credit comment) and ar_with_widen, which reruns once on a grid from −1 to 2 if the set touches the edge. It printed the full-sample sets for both cov types next to the Wald interval.
2. On its own initiative it added a gap check, because the starter's min and max assume the AR set is a single interval.
3. It asked whether subsamples should get both cov types or HC1 only. I did not answer in this conversation.
4. Cells 2 and 3 (subsample table, CSV, figure) were not written in this conversation.

**[Mechanism Check]** The model explained that "nonrobust" matches Card's conventional SEs, that with a weak instrument the AR set can be two separate pieces or unbounded, which min and max would hide, and that a set still touching the wide grid's edge should be called unbounded on that side. The starter note I pasted explains that AR never divides by the first stage. I did not ask separate check questions.

**[Evaluate]** Expected Wald interval about [0.024, 0.239]; the gap warning; the touches-edge flag after widening. I did not paste output back into the chat. When I ran the cell locally, it printed AR (nonrobust) [0.025, 0.284] and AR (HC1) [0.029, 0.281], neither touching the grid edge and with no gap warning, against the Wald interval [0.024, 0.239].

**Full transcript**

Full transcript: see ai-transcripts/section3_transcript.txt (unedited).

**What I changed, and why:**
- Required both nonrobust and HC1 AR sets for the full sample, to compare like with like against Card's conventional SEs.
- Required one grid widening before calling a set unbounded, so a narrow grid is not mistaken for the answer.
- None. I ran the cells as written, with no edits of my own.

### Entry: Exercise 3, own specification choices

- Model and version: Claude Opus 5.5 (Anthropic), claude.ai chat
- Date: 2026-11-20

**[Prep]** Same setup prompt and quick-reference block as Exercise 1. The specs came from my Section 2 ambiguities list.

**[Request]** "Exercise 3: my own specification choices. Same working style." Purpose: "test Card's third link. Does 0.132 depend on choices the paper made but didn't justify?" I asked for an iv_spec helper and specs (a) baseline, (b) plus fatheduc, motheduc, momdad14, where I said to "ALSO run the baseline on the same reduced sample. That separates "the controls changed it" from "the sample changed it". Report the rows lost.", (c) 1966-only geography, (d) 1976-only geography, (e) nearc2 as the instrument, (f) both instruments with an over-identification test, saved to output/ex3_specs.csv.

**[Iterate]**
1. The model gave a three-cell plan and wrote Cell 1 only: iv_spec, a small add() helper that appends a labeled row, and specs (a), (b0) baseline on the family sample, and (b) with family controls, printing rows lost.
2. It used a joint F-test on the instruments for fs_F, because t squared is only valid with one instrument and spec (f) has two.
3. It left sinmom14 out by default, as optional in my prompt.
4. Cells for (c) to (f), the over-identification test and the CSV were not written in this conversation. The optional MY_SPEC check was not discussed.

**[Mechanism Check]** The model explained that (a) vs (b0) isolates the sample effect and (b0) vs (b) isolates the controls effect, and that dropna inside iv_spec only matters for specs that add variables with missing values. I did not ask separate check questions.

**[Evaluate]** Expected row (a): n = 3,010, coefficient 0.1315, SE 0.0550, fs_F ≈ 13.3; rows lost printed. I did not paste output back into the chat. When I ran the cell locally, row (a) reproduced the baseline (n = 3,010, 0.1315, SE 0.0550, fs_F 13.26). It also printed 790 rows lost to family background, (b0) 0.0799 (SE 0.0744, fs_F 6.62, n = 2,220) and (b) 0.0829 (SE 0.0768, fs_F 6.95, n = 2,220).

**Full transcript**

Full transcript: see ai-transcripts/section3_transcript.txt (unedited).

**What I changed, and why:**
- Required the baseline on the family-background sample, to separate the effect of the controls from the effect of losing rows.
- None. I ran the cells as written, with no edits of my own.

### Entry: Exercise 4, exclusion restriction and placebo

- Model and version: Claude Opus 5.5 (Anthropic), claude.ai chat
- Date: 2026-11-20

**[Prep]** Same setup prompt and quick-reference block as Exercise 1.

**[Request]** "Exercise 4: the exclusion restriction, Card's key assumption. Same working style." Purpose: "test Card's fourth link. Does growing up near a college affect wages ONLY through schooling?" I asked for a placebo: "regress pre-determined ability measures on nearc4 + CTRL_LIST (NOT educ, which is the channel)", with IQ and KWW standardized on their estimation samples, reporting coef, SE, p-value and n, an explanation of why these are reasonable placebos and the caveat about schooling done by 1966, then 2SLS with IQ and/or KWW as controls, saved to output/ex4_placebo.csv and output/ex4_ability_controls.csv.

**[Iterate]**
1. The model flagged that exper and expersq in CTRL_LIST are built from educ (exper = age − educ − 6), so controlling for them in a placebo partly conditions on the channel. Cell 1 runs the placebo twice, with age and agesq in place of exper and expersq, and with CTRL_LIST as I asked, so I can see whether it matters.
2. It used HC1 SEs for the placebo regressions.
3. The ability-controls 2SLS cell, the optional low-parental-education subsample and the spec plot were not written in this conversation.

**[Mechanism Check]** The model explained the placebo logic and the caveat: the men were 14 to 24 in 1966, so older men had done much of their schooling, and a nearby college may also signal better local schools, so a nonzero coefficient is a warning sign, not proof. It said KWW was tested at the 1966 interview and IQ comes from school records collected around 1968; it also noted IQ's 949 missing values make its sample selected. I verified the timing against the NLS documentation (nlsinfo.org, "Aptitude, Achievement, and Intelligence Scores (Young Men's Cohort Only)"). KWW was administered at the 1966 interview. IQ was built from high-school test records (Kohen 1973), collected by the 1968 School Survey, so the tests predate college. The survey covered only men who had finished ninth grade by 1968 and signed a records waiver, which explains the missing IQ values. I did not ask separate check questions.

**[Evaluate]** I did not paste output back into the chat. When I ran the cell locally, nearc4 was not significant for IQ (0.059 SD, p = 0.22 with age controls, 0.044 SD, p = 0.32 with experience controls, n = 2,061). It was significant for KWW (0.088 SD, p = 0.019 with age controls, 0.111 SD, p = 0.005 with experience controls, n = 2,963). The two control sets give the same conclusion.

**Full transcript**

Full transcript: see ai-transcripts/section3_transcript.txt (unedited).

**What I changed, and why:**
- Required placebo outcomes measured before 1976 wages and standardized, so the coefficient reads in SDs.
- Kept both the age-control and the experience-control versions, as the notebook shows.

## Section 4: Does the finding hold?

No AI used in this section.

## Section 5: Peer audit

No AI used in this section.

## Section 6: Your own repository

### Entry: repository setup, troubleshooting and README

- Model and version: Claude Opus 5.5 (Anthropic), in Claude Code, a separate session from the Section 3 work
- Date: 2026-11-19 to 2026-11-20

**[Prep]** I used Claude Code in the course folder as a helper for the project logistics. I told it the brief and that Sections 1 and 2 are no AI, so for those it only pointed to my own output and gave hints, and it did not write or edit any Section 1 or 2 code or text.

**[Request]** Help with:
- understanding the deliverables and which files belong to the template, the course starter and me
- setting up git and pushing to GitHub
- fixing a notebook kernel that hung (the editor was using the course-folder venv instead of the repository's `.venv`)
- drafting the prompts I gave the Section 3 chat
- drafting `README.md`

**[Iterate]** For the README, it used the repository's files and the numbers produced by `python reproduce.py` (2SLS 0.1315 against Card's 0.132, a −0.38% gap, about 10 seconds' runtime). I asked it to add this disclosure entry.

**[Mechanism Check]** It explained why the kernel hung, why `Path("../output")` in notebook 02 writes outside the repository when `reproduce.py` runs from the root, and how the git remote was set.

**[Evaluate]** I checked the README's headline number against `output/headline_table.csv` and the runtime against a full `python reproduce.py` run. I checked the citation against the paper.

**Transcript:** not attached, because this session was troubleshooting and logistics rather than analysis. Available on request.

**What I changed, and why:** None.
