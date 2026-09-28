# The Unofficial Guide

<!-- Replace this line with your name and which corpus you picked. -->

> **This file is your submission.** Fill it in as you go — most sections get
> written during the milestone that produces them, not at the end.
>
> How the starter works, and every command you'll need, is in `RUNNING.md`.
> Leave that file alone.
>
> **Paste everything as text.** No screenshots, no video. A typed table gets
> full credit; a picture of the same table gets none, because the grader can't
> read it.
>
> Delete these instruction blocks as you replace them. The `<!-- -->` comments
> are notes to you and don't show up when the page renders — you can leave them
> or remove them.

---

# Week 1

## What This Does

This project is an unofficial searchable guide to campus life. It uses the `campus_life` corpus, which contains short student-style posts about housing, dining, courses, and university rules. A user can ask questions such as how the housing lottery works, when a dining hall is busiest, or how much work a course requires. The system retrieves relevant documents, answers using those documents, and names the source file it used.


## Chunking Strategy

**Chunk size:** One complete source document per chunk. The campus_life documents are short, ranging from 178 to 549 characters in the starter output.

**Overlap:** 0 characters.

I chose to keep each document as one chunk because the campus_life posts are short and usually focus on one complete topic, such as one course, residence hall, dining hall, or administrative policy. Splitting them into smaller character-based pieces could cut a useful sentence or separate related details. Keeping the full post together lets a retrieved chunk make sense on its own.


## Sample Chunks

<!-- Five chunks, pasted as text. Label each one and name the file it came from
     AND the function that produced it — the grader checks your code against
     what you claim here.

     `python app.py chunks -n 5` prints all three for you. Copy them straight
     across.
-->

**Chunk 1** — source: `admin_add_drop_deadline.txt#0` — produced by: `chunker.py::split_documents`

```text
On the add/drop deadline

You can add a course through the end of the second week. Dropping is a longer window — through the end of week six — but a drop after week two shows as a W on your transcript. Nothing anywhere on the registrar's site says this plainly, and students find out from each other.
```

**Chunk 2** — source: `course_biol_160.txt#0` — produced by: `chunker.py::split_documents`

```text
BIOL 160 Cell Biology

I lived here my sophomore year. Format is lecture three times a week with a weekly lab. Assessment: four unit tests and a cumulative final. Not curved.

Expect 9 to 11 hours a week, the heaviest first-year course by reputation.

The one piece of advice: the unit tests come fast, roughly every three weeks; falling behind once is very hard to recover from.
```

**Chunk 3** — source: `course_hist_118_workload.txt#0` — produced by: `chunker.py::split_documents`

```text
Workload for HIST 118 Modern World History

People keep asking so: a lot of reading, about 120 pages a week, but no problem sets. That's real time, not optimistic time.

It's front-loaded — the first month is heavier than the rest, partly because you're learning the format.
```

**Chunk 4** — source: `dining_pellew_dining_hall_followup.txt#0` — produced by: `chunker.py::split_documents`

```text
Re: Pellew Dining Hall

Adding to what people have said about Pellew Dining Hall. The wait figure of 12 to 18 minutes at peak matches what I've seen. If you're trying to eat between classes, go before 11:45 and it's a different building entirely.

Also worth saying: the furthest hall from anywhere, next to the athletics centre. Nobody tells you this at orientation.
```

**Chunk 5** — source: `housing_innisfree_hall.txt#0` — produced by: `chunker.py::split_documents`

```text
Innisfree Hall — what it's actually like

Transferred in last year, so take this with a grain of salt. Built 1991, renovated 2022. Rooms are doubles arranged as pairs sharing one bathroom between two rooms.

The good: the shared-bathroom-between-two-rooms arrangement is the best compromise on campus.

The bad: no air conditioning, which matters for the first three weeks of September.

Laundry costs $1.75 wash, $1.75 dry, app-based. On noise: moderate; the building is L-shaped and the short wing is much quieter.
```

## Sample Answer

<!-- One complete question and answer, pasted as text, with the source line
     visible. Milestone 4. -->

**Question:**

How does the housing lottery work for juniors and seniors?

**Answer:**

```text
For juniors and seniors, the housing lottery orders students by accumulated credit hours first, and ties are broken randomly (admin_housing_lottery.txt).

Sources retrieved: admin_housing_lottery.txt, admin_parking_permits.txt, advising_registration.txt, housing_old_brewhouse.txt, housing_tamsin_court.txt
```

**My relevance cutoff:**

I kept the relevance cutoff at `0.6`. My five in-corpus questions had best distances from `0.184` to `0.368`, while the five unrelated questions had best distances from `0.825` to `0.934`. The cutoff of `0.6` sits in the gap, so it accepts questions the campus_life corpus can answer and refuses questions from unrelated topics.

| Question | In corpus? | Best distance |
|---|---|---:|
| How does the housing lottery work for juniors and seniors? | Yes | 0.184 |
| When are the busiest lunch hours at Pellew Dining Hall? | Yes | 0.257 |
| What are CS 210 exams based on? | Yes | 0.297 |
| What are the benefits and drawbacks of living in Morrow House? | Yes | 0.368 |
| How much time outside class should I expect to spend on ECON 101? | Yes | 0.248 |
| What is the capital of Mongolia? | No | 0.825 |
| How do I change the oil in a diesel engine? | No | 0.934 |
| Who won the 1994 World Cup? | No | 0.886 |
| What is the recommended dosage of ibuprofen for a headache? | No | 0.844 |
| How do I write a for loop in Rust? | No | 0.896 |

## How I Used AI


**1.** I asked an AI assistant to explain the RAG system, chunking, retrieval distances, and reindexing in nontechnical language. It also helped me turn the documents I read into specific test questions instead ofbroad questions with no clear answer. I used the suggestions to create five questions in `questions.py` and chose short expected phrases that match the source documents.

**2.** I asked an AI assistant how to replace the starter's fixedsize chunking with a strategy that fits the short campus_life posts. It suggested keeping one complete document as one chunk. I added that strategy to `chunker.py`, fixed an indentation issue, reindexed the corpus, and used retrieval results to confirm that the correct housing-lottery document was returned first.


<!-- ── Stretch features ─────────────────────────────────────────────────────
     Doing one? Say so here BEFORE you start. A feature this README never
     claims earns nothing.
     ───────────────────────────────────────────────────────────────────────── -->

---

# Week 2

<!-- These sections get ADDED to what's already above. Don't delete or rewrite
     week 1 — the point is that someone can see what you said before you knew
     how it went. -->

## Run Log — Before

Produced by `run_eval.py::main` in `results/run_2026-09-27_1545_before.md`. Retrieval used `store.py::search`; chunks were produced by `chunker.py::split_documents`.

**Criteria 1, 2, and 5 — run 1 answers:**

```text
How does the housing lottery work for juniors and seniors?
For juniors and seniors, the housing lottery orders students by accumulated credit hours first, with ties broken randomly (admin_housing_lottery.txt).

When are the busiest lunch hours at Pellew Dining Hall?
The peak wait times at Pellew Dining Hall are from 11:45 to 12:30.

Source: dining_pellew_dining_hall.txt

What are CS 210 exams based on?
CS 210 exams are drawn from lecture material rather than the textbook (source: course_cs_210_exams.txt and course_cs_210.txt).

What are the benefits and drawbacks of living in Morrow House?
Based on housing_morrow_house.txt, the benefits of living in Morrow House are that it is in the cheapest housing tier by about $900 a year and the singles are real singles. The drawbacks are a known damp problem on the ground floor, it is loud until about 1am on weekends, and there are no enforced quiet hours.

How much time outside class should I expect to spend on ECON 101?
You should expect to spend 4 hours a week outside of class on ECON 101.

Sources:
- course_econ_101_workload.txt
- course_econ_101.txt
```

**Criterion 3 — relevance gate output:**

```text
refused  (best distance 0.825)  What is the capital of Mongolia?
refused  (best distance 0.934)  How do I change the oil in a diesel engine?
refused  (best distance 0.886)  Who won the 1994 World Cup?
refused  (best distance 0.844)  What is the recommended dosage of ibuprofen for a headache?
refused  (best distance 0.896)  How do I write a for loop in Rust?
-> gate refused 5 of 5
```

**Criterion 4 — chunk output:**

```text
88 chunks total. Showing 5, spread across the corpus.
Chunk 1 through Chunk 5 above were produced by chunker.py::split_documents.
Each sampled chunk contains a complete thought without beginning or ending in the middle of a sentence.
```

     Milestone 1. -->

| Criterion | Target | Run 1 | Run 2 | Run 3 | Verdict |
|---|---|---|---|---|---|
| 1. Retrieved chunk contains the answer | 4 of 5 | 5/5 | 5/5 | 5/5 | MET |
| 2. Every answer names a source | 5 of 5 | 5/5 | 5/5 | 5/5 | MET |
| 3. Gate stops out-of-corpus questions | 4 of 5 | 5/5 | 5/5 | 5/5 | MET |
| 4. Chunks are complete thoughts | 4 of 5 | 5/5 | 5/5 | 5/5 | MET |
| 5. Answers include the expected information | 4 of 5 | 5/5 | 5/5 | 5/5 | MET |

<!-- Underneath, paste the REAL output for each criterion from one of your
     runs — the actual text your system produced, not a description of it.
     Name the file and function that produced it. -->

## Verdicts

<!-- MET or MISSED for each of the five, against the target you wrote last
     week — not a new one. Plus a sentence on how you decided. That sentence
     matters most where it was close.

     If your target said 4 of 5 and your runs came out 4, 3, 4, that's a MISS.
     The target has to hold, not show up occasionally.

     Milestone 2. -->

| # | Criterion | Verdict | How I decided |
|---|---|---|---|
| 1 | Retrieved chunks contain the answer | MET | In every run, all five questions retrieved a chunk containing the answer. The correct source document was included for the housing lottery, Pellew Dining Hall, CS 210, Morrow House, and ECON 101 questions. |
| 2 | Every answer names a source | MET | All five answers in every run named at least one source filename. |
| 3 | The relevance gate stops out-of-corpus questions | MET | The gate refused all 5 of 5 unrelated questions. This was deterministic, so the same 5/5 result applies to all three run columns. |
| 4 | Chunks are complete thoughts | MET | All five sampled chunks were complete, readable thoughts and none began or ended in the middle of a sentence. |
| 5 | Answers include the expected information | MET | All five answers included their expected phrase: credit hours, 11:45, lecture material, cheapest, or 4 hours. |

## Diagnoses
No criteria were missed in the before run, so there is no failed pipeline stage to diagnose.

However, the results suggest that some of my targets were conservative. Criterion 1 required only 4 of 5 questions to retrieve a chunk containing the answer, but all five questions did so in every run. If I wrote this criterion again, I would tighten it to require 5 of 5 questions. I would keep the same measurement method because it was clear and repeatable.

I also noticed that retrieving five chunks sometimes included loosely related sources. For example, the housing-lottery question also retrieved parking, advising, and housing documents that were not needed for the answer. This did not cause a missed criterion, but it is a useful observation for the one improvement I will measure next.

     Milestone 3. -->

## The Improvement

**What I changed:**

I changed `TOP_K` in `config.py` from `5` to `3`. This reduced the number of chunks retrieved and sent to Gemini for each question. I did not change the corpus, chunking strategy, embedding model, threshold, or prompt.

**Why I picked it:**

The before run passed every criterion, but the retrieved source lists sometimes contained loosely related documents. For example, the housing-lottery question retrieved parking, advising, and unrelated housing documents. I reduced `TOP_K` to make the context more focused while keeping the closest relevant chunks.

### Run Log — After

| Criterion | Target | Run 1 | Run 2 | Run 3 | Verdict |
|---|---|---|---|---|---|
| 1. Retrieved chunk contains the answer | 4 of 5 | 5/5 | 5/5 | 5/5 | MET |
| 2. Every answer names a source | 5 of 5 | 5/5 | 5/5 | 5/5 | MET |
| 3. Gate stops out-of-corpus questions | 4 of 5 | 5/5 | 5/5 | 5/5 | MET |
| 4. Chunks are complete thoughts | 4 of 5 | 5/5 | 5/5 | 5/5 | MET |
| 5. Answers include the expected information | 4 of 5 | 5/5 | 5/5 | 5/5 | MET |

### Real output — after

Produced by `run_eval.py::main` in `results/run_2026-09-27_2004_after.md`. Retrieval used `store.py::search` with `TOP_K = 3`.

**Run 1 answers:**

```text
How does the housing lottery work for juniors and seniors?
For juniors and seniors, the housing lottery orders students by accumulated credit hours first, and uses random tie-breaking only when needed.

Source: admin_housing_lottery.txt

When are the busiest lunch hours at Pellew Dining Hall?
The peak lunch wait times at Pellew Dining Hall are from 11:45 to 12:30.

Source: dining_pellew_dining_hall.txt

What are CS 210 exams based on?
CS 210 exams are drawn from lecture material rather than the textbook, and they reuse the lab problems.

Source: course_cs_210_exams.txt

What are the benefits and drawbacks of living in Morrow House?
The benefits of living in Morrow House are that it is the cheapest housing tier by about $900 a year and its single rooms are real singles. The drawbacks are a known damp problem on the ground floor, loud weekends until about 1am, and no enforced quiet hours.

Source: housing_morrow_house.txt

How much time outside class should I expect to spend on ECON 101?
You should expect to spend 4 hours a week outside of class on ECON 101.

Sources: course_econ_101_workload.txt and course_econ_101.txt
```

**Gate output:**

```text
-> gate refused 5 of 5 out-of-scope questions
```

**Did it help?**

Yes, the improvement helped make the retrieved context more focused. All five criteria remained MET at 5/5, so the change did not reduce answer coverage, source attribution, or gate performance. The after run used three chunks instead of five, which removed some unrelated sources: the Morrow House question retrieved only Morrow House documents, and the ECON 101 question retrieved fewer unrelated course documents. The best-distance scores did not change because `TOP_K` changes the number of returned chunks, not the similarity score of the best chunk.

     Milestone 4. -->

## What's Still Broken

No criteria were missed after the improvement. However, some retrieval results still include at least one loosely related document. For example, the housing-lottery question still retrieved advising and Tamsin Court documents alongside the correct housing-lottery document. If I continued, I would test hybrid search that combines meaning-based search with keyword search, because course names and exact terms may benefit from keyword matching. I stopped here because the project requires one measured improvement, and the reduction from five chunks to three already improved context focus without reducing any criterion score.

     Milestone 5. -->

## What I'd Do Differently

If I wrote the criteria again, I would make criterion 1 stricter: for all 5 of 5 questions, one of the top three retrieved chunks should contain the answer. My original target of 4 of 5 was measurable, but the before run passed it easily. Requiring the answer in the top three results would better measure whether retrieval is focused rather than only whether the answer appears somewhere in a larger set of results.

## How I Used AI

In Unit 2, I asked an AI assistant to help me compare the before and after evaluation output. It pointed out that the numerical scores stayed the same but that fewer unrelated source files were retrieved after I changed `TOP_K` from 5 to 3. I checked the source lists in both result files myself and used that comparison to explain whether the improvement helped.

     Milestone 5. -->
