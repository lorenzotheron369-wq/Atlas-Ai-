# Atlas: rules for any coding agent

## Context
Atlas is a personal Spanish-learning app for one learner, Lorenzo.
Lorenzo is not a programmer and cannot review code. Every rule below
exists because of that.

## Hard rules
- Everything lives in ONE file: atlas-lesson-001.html.
  Plain HTML, CSS and JavaScript only. No frameworks, no npm, no build
  step, no server, no database, no user accounts.
- Never put API keys, tokens or secrets in any file.
- Make the smallest change that completes the task. Do not rewrite,
  reformat, rename or restyle code the task does not require touching.
- All existing screens must keep working exactly as before.
- Keep the existing visual design: use the CSS variables in :root and
  the existing component styles (.frame, .cta, .chip, .note, etc.).
- No streaks, XP, points, badges or leaderboards.
- Spanish is Castilian (es-ES). Do not write new Spanish sentences
  unless the task explicitly asks for it. If you add any Spanish at all,
  list every new Spanish string at the end of your reply so a native
  speaker can check it.
- Storage must go through the existing `store` helper and must still
  work when storage is unavailable.
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
