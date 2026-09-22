# Atlas: rules for any coding agent

## Context
Atlas is a personal Spanish-learning app for one learner, Lorenzo.
He is a complete beginner (zero Spanish). He is not a programmer and
cannot review code. Every rule below exists because of that.

## Hard rules
- Everything lives in ONE file: atlas-lesson-001.html.
  Plain HTML, CSS and JavaScript only. No frameworks, no npm, no build
  step, no server, no database, no user accounts.
- Never put API keys, tokens or secrets in any file.
- Make the smallest change that completes the task. Do not rewrite,
  reformat, rename or restyle code the task does not require touching.
- Existing screens must keep working unless the task says to change them.
- Keep the existing visual design: use the CSS variables in :root and
  the existing component styles (.frame, .cta, .chip, .note, etc.).
- No streaks, XP, points, badges or leaderboards. A 14-day completion
  grid is allowed, but it must never show a streak count or a
  "broken" state.

## Language rules
- Spanish is MEXICAN Spanish (es-MX). Use the polite "usted" form when
  speaking to service staff (waiters, shop staff, reception).
- Do not write new Spanish unless the task gives it to you word for
  word. If you add or change ANY Spanish, list every Spanish string at
  the end of your reply so a native Mexican speaker can check it.

## Beginner rules
- Never show a Spanish word before it has been taught, unless the task
  marks it as new, and then show its English meaning next to it.
- In lesson 001, every Spanish line on screen shows its English meaning.
- All interface text (buttons, headings, instructions, labels) is in
  plain English. Spanish appears only as learning content.
- Every screen tells the learner, in one short sentence, what to do now.
- The learner must always be able to get back to the home screen.

## Storage
- Storage must go through the `store` helper (which uses
  window.storage, then localStorage, then memory). Never call
  localStorage or window.storage directly anywhere else.

## Saving work
- Do not commit or push anything. Lorenzo makes his own save-point
  after he has tested the change.

## Before you finish
1. Check the JavaScript has no syntax errors (for example, extract the
   <script> contents and run `node --check` on it).
2. Reply in plain English, no jargon:
   - What changed, in 3-5 sentences.
   - How to test it, as numbered click-by-click steps, on a laptop AND
     on a phone.
   - What could break, and what it would look like if it did.
