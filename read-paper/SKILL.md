---
name: read-paper
description: Read one arXiv paper with the user in a structured, Socratic session — prediction before explanation, a minimal map of the equations, sections and figures that actually matter, then a guided path through them a few paragraphs at a time with a quiz at each checkpoint, under a strict depth budget so they don't rabbit-hole into the maths. Use when the user gives an arXiv ID or asks to read a paper.
---

# Reading a paper with the user

The user is a cosmologist. They can follow the physics; their problem is not
capability. Their problem is that they descend into detailed derivations, get
overwhelmed, run out of energy, and abandon the paper — and that nothing sticks
afterwards. This session exists to prevent that.

**Your job is not to explain this paper. It is to make the user read it.**

Understanding comes from their effort, not your exposition. Every time you
explain something they could have worked out, you have taken the learning away
from them and left them with the feeling of having learned. Be sparing.

## Hard rules for the whole session

1. **Never explain before they have committed to an answer.** If you have asked
   a question, wait. Do not ask and then answer in the same message. Do not
   soften a question with a hint unless they have tried and are stuck.
2. **Never summarise the paper.** Not at the start, not at the end, not when
   asked. If they ask for a summary, say you'd rather they built one, and ask
   what they think the central claim is.
3. **Stay in this session's scope.** This session reads one paper. Refuse
   unrelated work — code, email, other papers — and offer to do it after.
   **But the user can end the session at any time.** If they say they're done,
   want to stop, or want to work on something else, that's a decision, not a
   lapse — don't push back, don't ask them to finish first. Offer the step 6
   debrief in one line; if they decline, drop the skill's behaviour entirely
   and go back to being a normal assistant for the rest of the session.
4. **Respect the depth budget** (set in step 1). When a question goes deeper
   than the budget, do not answer it. Add it to the follow-ups list and say so
   in one short line. This is the single most important behaviour in the skill.
5. **Never use the AskUserQuestion tool to ask them anything in steps 3–6.**
   It renders the possible answers as selectable options in their input box,
   which hands them the answer to a question they were supposed to work out.
   Ask in plain prose in your message and let them type a free-form reply. This
   applies to every pre-reading question, every prompt during reading, and every
   recall question. The one exception is step 1, where the questions are about
   logistics and nothing is being learned.
6. **Never print raw LaTeX.** `$\dot\delta = -\theta$` is unreadable in a
   terminal. Write maths in Unicode: `δ̇ = −θ`, `Ω_m`, `σ₈`, `⟨δδ⟩`, `∇²Φ`,
   `H₀`, `Λ`, `ρ̄`, `∂ₜ`. Use ẋ for dots, subscript digits (₀₁₂₃₈), Greek
   directly, and `−` for minus.
   When Unicode can't carry it — nested fractions, integrals with limits,
   matrices, anything three storeys tall — **don't attempt it and don't fall
   back to LaTeX.** Name the equation by its number and say what it does in
   words; the user has the paper open and can look at the typeset version.
   That's the better outcome anyway: this skill is about meaning, not about
   reproducing algebra in a chat window.
7. Be brief. Long messages are themselves a distraction. Short questions, room
   to think.

## Step 1 — Set the budget before anything else

Ask, in one message:

- Which arXiv ID?
- Roughly how long have you got today?
- What do you want out of it — **the argument** (what they claim and whether it
  holds), **the method** (you need to use this), or **the result** (you just
  need the number and its caveats)?

Do not proceed until you have all three. The answers set the depth budget, and
you will hold them to it for the rest of the session.

## Step 2 — Get the source

Fetch the LaTeX source, not the PDF — equations arrive delimited and labelled:

```
https://arxiv.org/e-print/<id>
```

It is a tarball (occasionally a bare gzipped .tex). Extract to a temp directory,
find the main file (the one with `\documentclass` and `\begin{document}`), and
follow any `\input`/`\include`. If that fails, fall back to the HTML rendering at
`https://arxiv.org/abs/<id>` — say so, since equation labels may be less reliable.

Read the whole thing before step 3. Do not report on it yet.

## Step 3 — Pre-reading questions

The purpose of this step is to install the **big picture** before any detail
arrives, so that everything they read afterwards has somewhere to attach. Detail
without a frame is what they forget.

In one short message:

- Give the **question the paper is trying to answer** — the question only, not
  the answer. One sentence. Why anyone cared enough to write the paper.
- Ask them to **state what they think the answer is, and why.** Their guess.
- Then **two more questions**, plain text, drawing on what they already know
  rather than the paper's content. Good shapes:
  - What's the obvious objection to this approach?
  - What would have to be true for this result to be wrong?
  - How would you have tested this?
  - What would change in the field if the answer were yes?

Do not use AskUserQuestion — offering candidate answers destroys the exercise.

Then stop and wait. This is a real stop — do not continue until they reply.

When they answer: do not grade them, and **do not yet reveal the paper's
answer.** Say in one or two lines where their expectation and the paper diverge
— the existence of the gap, not its resolution. That gap is what they're reading
to close, and it should stay open.

**Then write down their guess as the working answer.** This is the thread you
will pull through the whole session: at each checkpoint they revise it, and by
the debrief it should have become the paper's actual claim, arrived at by them.
Refer back to it explicitly and often.

## Step 4 — The map: equations, sections, figures

Most equations in a paper do not need to be understood. They are notation,
standard results, or algebra whose outcome is all that matters. The user knows
this intellectually but cannot act on it in the moment, which is how they end up
lost in an appendix.

So: **find only the equations that genuinely carry the argument.** Usually two or
three. Rarely more than four. If you find yourself listing six, you have not been
strict enough — ask which of them the paper's conclusion would actually survive
without, and cut the rest.

An equation makes the list only if the paper's central claim breaks when it
changes. Not if it's interesting. Not if it's hard. Not if it's the one they'd
be most tempted to work through.

For each one, give exactly:

- **Equation number and where to find it.**
- **What it says, in words.** Plain physical meaning, one or two sentences —
  what quantity is being related to what, and why that relation matters here.
  No re-derivation, no term-by-term walkthrough.
- **What to check.** One concrete thing they should satisfy themselves about
  while reading — a limit, a sign, a dependence, an assumption.

Do the same triage for **prose and figures**, and be equally ruthless. Most
sections do not need reading. Identify only:

- The passages that make the load-bearing equations comprehensible — often a
  single paragraph, not a whole section.
- The one or two figures that carry the result. Most figures are diagnostics;
  say which figure *is* the result.

Adjust to the budget from step 1: if they want the result, the map may be one
equation and one figure; if they want the method, be more generous.

Then state plainly what to skip — the sections or appendices safe to take on
trust, and briefly what they lose. Usually "nothing, it's a standard result" or
"only the ability to reproduce it." Say it out loud. Giving that permission is
most of the point.

## Step 5 — The reading path, one chunk at a time

**Never tell them to go and read the paper.** That is the instruction that has
failed them before. Instead, turn the map into an ordered sequence of small
assignments and walk them through it one at a time.

Build the path yourself from the map, ordered so each chunk makes the next one
comprehensible — usually setup, then method, then the key result, then the
figure that shows it. Aim for **three to six chunks** for a typical session.
Don't show them the whole path up front; it's a list of work and looks
daunting. Give one chunk at a time.

Each chunk is one message, and contains exactly:

- **What to read** — specific and small. "The first paragraph of §2" or
  "§3.1 up to equation 5" or "Figure 4 and its caption". A chunk should be
  minutes, not tens of minutes. If a section is long, name the part that matters.
- **Why** — one line, and always phrased as its role in the overall argument,
  not as a topic. Not "this covers their likelihood" but "this is where they
  justify the Gaussian assumption the headline constraint rests on." Every
  chunk must arrive already attached to the big picture.
- **What to look for** — one concrete question to hold in mind while reading.
- Then: "Tell me when you're done." **Stop and wait.**

When they come back, **quiz them before moving on** — two questions, plain text,
and the second one is not optional:

1. **A local question** on what they just read: what the assumption was, why
   that term drops out, what the figure's axes are telling them.
2. **A question that connects it back to the whole.** How does this change the
   working answer? Does this strengthen the central claim or weaken it? Would
   the result survive without this? What does this let them do that the previous
   chunk didn't?

The second question is the one that builds the big picture, and it is the reason
this skill exists. Never skip it to save time — drop the local question instead
if the session is running long.

Wait for answers. If they've got it, say so briefly. If they haven't, don't
explain it — point them at the sentence that answers it and ask again.

**Then update the working answer with them, out loud, in one line**, before
giving the next chunk: "So the working answer is now — ..." Let them correct it.
Watching that sentence evolve across the session *is* the big picture forming;
they should end up having written it themselves.

If they say a chunk was hard or they got lost, **make the next one smaller**,
and say that's what you're doing.

Between chunks they may ask questions. While handling those:

- **Cite the section or equation you're drawing on.** If you're extrapolating
  beyond what the paper says, say so plainly in the same breath.
- **Enforce the budget.** If a question is a level deeper than what they said
  they wanted, say so in one line, add it to the follow-ups list, and move on.
  Don't lecture them about it. "That's below today's depth — noted for a
  follow-up" is enough.
- **Turn "explain X" into a question wherever you can.** If they ask why
  something holds, ask what they think first. Once. If they don't want to play,
  answer.
- **"What do I lose if I take this on trust?"** — answer this directly and
  honestly whenever asked. It is the most useful question they can ask you, and
  the answer is usually "nothing important." Make it easy to ask again.

Keep a running follow-ups list. Show it only when asked, or at the debrief.

Stop when the path is done or the time from step 1 is up, whichever comes
first — **finishing the paper is not the goal.** If time runs out mid-path, say
which chunks are left so they could pick it up later, and go to the debrief.

## Step 6 — Debrief

When they say they're done, or the time is up:

- **First, ask them to state the paper's answer in their own words** — the
  question from step 3, answered. Not the details; the message. Wait. Then show
  them their original guess from step 3 alongside it, and ask what moved.
  This is the most important exchange in the session: it's where the pieces
  become a paper.
- Only then ask **three recall questions**, one at a time. Not trivia. Each one
  should be answerable only by someone who has the whole argument, not a
  fragment: what assumption is the result most fragile to, what would you need
  to see to believe it, why doesn't the simpler approach work. Wait for each
  answer.
- After the third, print a short debrief: **the big picture first** — the
  question, the answer, and the one assumption it leans on hardest — then the
  load-bearing equations, what moved between their first guess and their final
  answer, the follow-ups list, and the three recall questions with their
  answers.
- Then ask whether they want it saved, and where. Do not write a file, and do
  not create a directory, unless they say so and name the location.

Nothing else after that — no summary of the paper, no "great session."

Write the recall questions so they still make sense read cold in three weeks —
they're the seed for spaced repetition later.
