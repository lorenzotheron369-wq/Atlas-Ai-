# ATLAS — Master Build File

Single source of truth. AGENTS.md says HOW to work. This file says
WHAT to build and WHY. Re-read both before every task.

---

## 1. What Atlas is

A daily 15-45 minute Spanish session for ONE adult beginner, Lorenzo,
running in a browser from a single HTML file.

Not a platform. Not multi-user. Not four languages. Not a mobile app.
Not a business. One learner, one language, one file.

## 2. The learner

- 30, native English + Afrikaans, based in South Africa.
- Complete beginner in Spanish. Zero. Assume nothing is known.
- 45 min/day normal, up to 90 on off days. ADHD-pattern attention.
- Not a programmer. Cannot read or review code. This is the single
  most important constraint in the project.

## 3. The one metric

Days out of the last 14 that a session was completed.

Not vocabulary count, not XP, not lessons shipped. Below 10 means the
session design is wrong; adding features will not fix it.

Second metric, monthly: record a 60-second unscripted attempt at that
month's real-world scenario. Keep the recordings.

## 4. Dialect: MEXICAN Spanish (es-MX)

Changed from Spain Spanish. Consequences already applied:
- Voice order: es-MX, then es-US, then es-419, then any es.
- Polite "usted" with service staff: "¿Tiene...?" not "¿Tienes...?"
- Currency is pesos, not euros.
- "caña" (small draft beer) is Spain-only. Removed.
- "tortilla" means flatbread in Mexico, not potato omelette. Removed
  from the bar scene.

## 5. Pedagogy — five rules

1. Frames over words. Sentence patterns with swappable slots
   ("___, por favor"). Patterns compound; word lists don't.
2. Comprehensible input built ONLY from material already taught,
   plus a little that context makes obvious. Anything new is labelled.
3. Speak every session, from day one. No exceptions.
4. Review due items FIRST, before new material. Non-negotiable.
5. Scenarios, not grammar topics. The unit is "ordering a beer",
   not "the present tense".

Pimsleur method, added: anticipation (ask, pause, answer), backward
build-up (teach long phrases from the end), and graduated repeats
within a session (~5 s, 25 s, 2 min, 10 min).
Never use the word "Pimsleur" in anything the user sees.

## 6. Deleted on purpose — do not re-add

- Streaks, XP, points, badges, levels, leaderboards. A streak is a
  punishment mechanic in a reward costume. The 14-day grid replaces
  it and must never show a streak count or a "broken" state.
- Spanish interface text. Buttons and instructions are plain English;
  Spanish appears only as learning content. "Pista · solo audio" was
  renamed "Audio drill" because the learner couldn't tell what it did.
- Tap-to-reveal English in lesson 001. A beginner needs the meaning
  visible.
- The old Spain-Spanish lesson content. Keep it commented out at the
  bottom of the script, labelled "OLD SPAIN-SPANISH CONTENT, NOT IN
  USE", for later rewriting.

## 7. What is NOT AI

Three things were wrongly specified as AI. Build them as plain code:
- Spaced repetition = SM-2 arithmetic, ~40 lines. Must be
  deterministic: same input, same answer, every time.
- Session planner = if/else rules on minutes available and items due.
- Progress tracking = reading stored data.

AI earns its place in only three places, all later: conversation
practice, batch content generation (offline, native-speaker reviewed,
then frozen), and phoneme-level pronunciation scoring via a
specialist API.

Current speaking score is WORD MATCH from browser speech recognition,
not pronunciation. It must always be labelled as such on screen.

## 8. Language order (revised)

1. Spanish - highest utility, priority goal.
2. Dutch - Afrikaans makes it near-free. ~150-250 hrs, not ~600.
3. Portuguese - deferred. Running it alongside beginner Spanish
   creates interference ("portuñol"), worst when neither is stable.
   Starts only when Spanish output is stable.
4. Arabic - blocked on a DECISION, not engineering. MSA is formal and
   written; dialects (Egyptian, Levantine, Gulf, Maghrebi) are
   effectively different products. Also right-to-left, which mirrors
   the whole interface. Pick a dialect before any Arabic work.

## 9. Deferred, with the trigger that unlocks each

| Deferred | Un-defer when |
|---|---|
| Real app (Next.js + Supabase) | Progress needed on phone AND laptop, OR a secret API key is required, OR 12 lessons outgrow one file |
| Second language | 10/14 days sustained for 8 weeks |
| AI conversation coach | 30+ phrases produced reliably and scripted speaking feels too easy |
| Adaptive session planner | 60+ logged sessions exist to adapt from |
| Real recorded audio / offline drill | Browser speech proves too limiting in daily use |
| Azure pronunciation scoring | Moving to a real app (needs a secret key) |
| Generated content | Hand-curated content runs out, ~12 scenarios |
| Other users | Lorenzo is at B1, using it daily, unprompted |

Rule: if you cannot name the evidence that would tell you to build it,
you are not ready to build it.

## 10. Known limits — state them, don't hide them

- Saved progress lives in ONE browser on ONE device. Phone and laptop
  do not sync. This is expected until the real app.
- Phone browsers stop speech when the screen locks. Wake Lock helps;
  true pocket-audio needs recorded files.
- Browser speech recognition needs Chrome or Edge. Elsewhere, fall
  back to self-rating.
- Lorenzo cannot QA Spanish. Every Spanish string in this file must be
  checked by a native Mexican speaker before it is drilled for a week.

---

# TASK LIST

Execute ONE task per session. Stop after each. Do not start the next
task until told. Lorenzo tests and makes a backup between tasks.

- [x] TASK 1 - Fix saving
- [x] TASK 2 - Beginner lesson + Mexican Spanish
- [x] TASK 3 - Home screen + navigation
- [x] TASK 4 - Audio drill

Tick a box only after Lorenzo confirms the task works.

---

## TASK 1 - Fix saving

The `store` helper only writes to window.storage, which does not exist
in a normal browser, so progress is lost on every reload.

Change ONLY the `store` helper:
1. Use window.storage if it exists (keep current behaviour).
2. Otherwise use localStorage, wrapped in try/catch.
3. Fall back to the in-memory `mem` object if both fail.
Same get/set signatures, same keys. Change nothing else.

Test: finish a lesson, reload, confirm the masthead shows "last today"
and scheduled items still appear.

---

## TASK 2 - Beginner lesson + Mexican Spanish

The current lesson is too hard: it plays a full conversation before
teaching any words, and hides the English. Rebuild it. Use ONLY the
Spanish given below, word for word.

**2.1 Voice and dialect**
- pickVoice(): prefer 'es-MX', then 'es-US', then 'es-419', then any
  voice starting with 'es'.
- say(): u.lang = 'es-MX', default rate 0.8.
- listen(): r.lang = 'es-MX'.

**2.2 New order**
STAGES = ['Learn','Build','Recall','Speak','Listen'].
Intro -> Learn -> Build -> Recall -> Speak -> Listen -> summary.
The conversation moves to LAST, as the payoff, once every word in it
is known.

**2.3 Intro screen**
- Eyebrow: "Lesson 001 · about 15 min"
- Heading: "Your first beer"
- Paragraph: "By the end of this lesson you can order a beer, ask the
  price, pay, and find the bathroom in Spanish."
- Dim paragraph: "You'll learn 8 words and phrases, then combine them
  into sentences. No experience needed."
- Button: "Start"

**2.4 LEARN screen (step 1)**
ONE card at a time, not a list. Each card: large emoji, large Spanish,
English underneath, audio plays automatically on appear.
Buttons "Hear it again" (ghost) and "Next". Counter "3 of 8".
Instruction: "Listen, then say it out loud."

| emoji | Spanish | English |
|---|---|---|
| 👋 | Hola | Hello |
| 🍺 | una cerveza | a beer |
| 💧 | agua | water |
| 🙏 | por favor | please |
| 😊 | gracias | thank you |
| 🧾 | la cuenta | the bill |
| 💰 | ¿Cuánto es? | How much is it? |
| 🚻 | ¿Dónde está el baño? | Where is the bathroom? |

**2.5 BUILD screen (step 2)** - replace FRAMES with exactly two:
- Pattern 1: "___, por favor." / "___, please."
  slots: una cerveza / a beer; agua / water; la cuenta / the bill
- Pattern 2: "¿Tiene ___?" / "Do you have ___?"
  slots: cerveza / beer; agua / water
Under pattern 2, a .note: "Tiene = do you have. It's the polite form
you use with waiters."
Instruction: "Tap a word to swap it in. The pattern stays the same."
Replace the old "memorise four frames" note with: "Two patterns, five
sentences. That's how you build a language."

**2.6 RECALL screen (step 3)** - replace RECALL with:
| English | Spanish |
|---|---|
| A beer, please. | Una cerveza, por favor. |
| Water, please. | Agua, por favor. |
| The bill, please. | La cuenta, por favor. |
| How much is it? | ¿Cuánto es? |
| Where is the bathroom? | ¿Dónde está el baño? |
| Do you have beer? | ¿Tiene cerveza? |
Instruction: "Say it in Spanish out loud first, then tap Show answer."

**2.7 SPEAK screen (step 4)** - replace MIRROR with:
Una cerveza, por favor. / ¿Cuánto es? / ¿Dónde está el baño? /
La cuenta, por favor.
Mic button: "🎙 Tap, then speak". "Hear it again" plays at rate 0.7.

**2.8 LISTEN screen (step 5)** - replace SCENE with the lines below.
Show English under EVERY line by default (remove tap-to-reveal here).
Lines marked NEW get a small "new" label (mono, brass-dim).
Heading "A bar in Mexico City". Instruction: "You know almost every
word here. Press Play all and follow along."

| who | Spanish | English | |
|---|---|---|---|
| — | Hola. | Hello. | |
| Tú | Hola. Una cerveza, por favor. | Hello. A beer, please. | |
| — | Sí. ¿Algo más? | Yes. Anything else? | NEW |
| Tú | Agua, por favor. | Water, please. | |
| — | ¿Algo más? | Anything else? | NEW |
| Tú | No, gracias. La cuenta, por favor. | No, thanks. The bill, please. | |
| — | Son cien pesos. | That's a hundred pesos. | NEW |
| Tú | Gracias. ¿Dónde está el baño? | Thanks. Where is the bathroom? | |
| — | Allá. | Over there. | NEW |

**2.9 Summary** - milestone text becomes "You can order a beer, pay,
and find the bathroom." Leave the rest; Task 3 redesigns it.

**2.10 Clean up**
- Remove the unused ESSENTIALS array.
- On boot, after loading saved review data, delete any entry whose
  phrase is not in the new RECALL or MIRROR lists, so old
  Spain-Spanish phrases never resurface.
- Move old FRAMES/RECALL/MIRROR/SCENE into one commented-out block at
  the bottom labelled "OLD SPAIN-SPANISH CONTENT, NOT IN USE".

List every Spanish string in the final file at the end of your reply.

---

## TASK 3 - Home screen + navigation

The learner finishes and doesn't know what happens next or how to move
around. Fix both.

**3.1 Home screen ("Today")** - the app opens here now, not the intro.
- Heading: "Today"
- 14-day grid: 14 small squares, oldest left, today right. Filled
  (brass) = a lesson or review was completed that day. Label:
  "X of the last 14 days". Never mention streaks.
- Review card (only when items are due): "Review · N phrases · about
  M min" (M = N/3 rounded up, minimum 1). Text: "Do this first. It's
  what makes words stick." Button "Start review".
- Lesson 001 card: "Lesson 001 · Your first beer". Status "Not
  started" or "Done ✓ · practise again anytime". Button "Start lesson"
  or "Practise again". If a review is due, make this button ghost
  style so review is clearly first.
- Lesson 002 card: "Coming soon", greyed, no button.
- If nothing due and something was done today: "You're done for today.
  Come back tomorrow for your review."
- Save completed days under 'atlas:es:days' (array of YYYY-MM-DD local
  dates) and lesson status under 'atlas:es:lessons'
  (e.g. {"001":{done:true, at:<timestamp>}}), via the store helper.

**3.2 Review session**
- Existing recall card design, one phrase at a time, items due now,
  maximum 20.
- Review keys start 'r:' or 'm:' followed by the Spanish. Show each
  phrase once even if it has both keys; apply the rating to all keys
  for that phrase.
- Look up the English from the lesson content.
- Hide the 5-step rail. Eyebrow: "Review · 3 of 8".
- On finish: mark today completed, return Home with a .note at the
  top: "Review done."

**3.3 Getting back**
- Tapping the "Atlas." wordmark always goes Home.
- During a lesson, review or drill, show a small "✕ Exit" text button
  on the right of the masthead in place of the meta line. Exit uses
  confirm(): "Leave now? Progress in this lesson won't be saved."

**3.4 Plain-English interface**
Empezar -> Start; Continuar -> Next; Siguiente marco -> Next pattern;
Escuchar -> Hear it; Play all -> Play all; Mostrar -> Show answer;
Otra vez -> Didn't know it; Difícil -> Hard; Bien -> Knew it;
Escuchar otra vez -> Hear it again; Intentar otra vez -> Try again;
Siguiente -> Next; Terminar -> Finish; Costó -> Struggled;
Salió bien -> Nailed it; ● Escuchando… -> ● Listening…
Eyebrows: "Step 1 of 5 · Learn the words", "Step 2 of 5 · Build
sentences", "Step 3 of 5 · Recall", "Step 4 of 5 · Speak",
"Step 5 of 5 · Listen".
Spanish learning content stays unchanged.

**3.5 Replace the summary screen**
- Heading "Lesson complete."
- Milestone box (.unlock style): "You can now order a beer, pay, and
  find the bathroom."
- Three stats: "Phrases you knew straight away: X of 6" (rated "Knew
  it"); "Speaking: X of 4 came through clearly" (word match 85%+);
  "Time: N min".
- "What happens next", numbered:
  1. "Tomorrow, open Atlas. You'll see a short review of N phrases
     (about M min)."
  2. "Do the review first. It's what makes these words stick."
  3. "Then practise this lesson again. Lesson 002 is coming soon."
- Small list "Coming back for review": up to 7 phrases with "later
  today" / "tomorrow" / "in N days".
- One primary button "Back to Today". Remove "Repetir la lección".
- Mark lesson 001 done and today completed.

Do not change any Spanish in this task.

---

## TASK 4 - Audio drill

Hands-free, audio-only practice. Never show the word "Pimsleur".

**4.1 Entry**
- Home card: "Audio drill · hands-free". Text: "Your phone asks, you
  answer out loud, then you hear the right answer. No tapping needed."
  Button "Start audio drill". Only visible once lesson 001 is done.
- Hide the 5-step rail. Show "✕ Exit" as in lessons.

**4.2 Content** - ONLY existing lesson content (Learn words, Build
sentences, RECALL). No new Spanish. Due review items go first.

**4.3 The loop, per item**
1. English voice: "How do you say: <English>?" (prefer en-GB or en-US,
   then any 'en' voice).
2. Silent pause = 2.5 s + 0.6 s per Spanish word.
3. Spanish at rate 0.8, short gap, again at rate 0.7.
4. Short gap, continue automatically.

**4.4 Backward build-up** - first appearance of any item of 3+ words,
before the loop, split on WORD boundaries only, never inside a word:
"favor" -> "por favor" -> "cerveza, por favor" ->
"Una cerveza, por favor."
Spanish voice rate 0.75, pause after each chunk = 1.5 s + 0.5 s per
word.

**4.5 Repeats within the session**
- Bring each item back ~5 s, 25 s, 2 min, 10 min after its previous
  appearance.
- Queue ordered by due time in seconds since session start. Play the
  most overdue; if nothing is due, introduce the next new item.
- Item finished after its 10-minute repeat.
- Session ends at 15 min or when all items finish, whichever first.
  Finish the current item before ending.

**4.6 Controls**
- After "Start", no taps needed.
- Show current Spanish in .frame style (glanceable), elapsed-time
  counter, two large buttons: "Pause"/"Resume" and "Finish".
- Pause stops speech instantly; Resume restarts the same item.
- Screen Wake Lock to keep the screen on; re-request when the page
  becomes visible. If unsupported, .note: "Keep your screen on
  manually."
- Start-screen .note: "Keep the screen on. Audio stops if your phone
  locks."

**4.7 No scoring** - no microphone, no self-rating, and do NOT change
the review schedule from this mode. On finish: mark today completed,
save under 'atlas:es:lastSession' with mode:'drill', then show minutes
practised, phrases heard, and "Back to Today".

**4.8 Acceptance checks**
- Lessons, reviews and Home behave exactly as before.
- Drill runs start to finish with zero taps after Start.
- A 4-word phrase gets build-up once, then the loop.
- The same phrase audibly returns at ~5 s, 25 s, 2 min, 10 min.
- Pause stops audio instantly; Resume continues.
- No new Spanish strings added.

---

## After Task 4

Do not build anything else without a trigger from section 9. The next
real work is not code: it is a spreadsheet of 12 scenarios x ~40
items, sourced by frequency and reviewed by a native Mexican speaker.
That phase is the actual product, and it is the one people skip.
