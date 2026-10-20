# Onboarding: Shreya S Salian

## 1. What is Tenali?

Tenali is a web-based math practice platform for school students, named after the courtier Tenali Raman. Instead of using a fixed question bank, it generates every problem with algorithms, so students don't see the same question twice. It covers 93 puzzle types across 69 topic areas, from basic addition to calculus, and every question comes with a step-by-step solution. Difficulty adapts to how the student is doing, so beginners aren't overwhelmed and strong students stay challenged. It also has game elements, like a live multiplayer Battle Arena and "misconception monsters" that help students fix common mistakes.

## 2. What do you understand by Tenali as a system?

Tenali has a React client and an Express server.

- **Server:** it is split into 19 route modules in `server/routes/`. Each puzzle type is an API with two endpoints: `GET /<type>-api/question` generates a question, and `POST /<type>-api/check` checks the answer.
- **Client:** it is built with Vite, and during development Vite forwards the `*-api` calls to Express.
- **Home grid:** tiles are defined as data in `client/src/features/tiles.js`. Each tile has a `key`, and `App.jsx` maps that key to a screen through `modeMap`. When I added the Mensuration Lab tile, I only added one line to `tiles.js`, because the `mensuration-lab` key already existed in `modeMap`.
- **Other parts:** data goes into MongoDB through Mongoose, the live Battle Arena uses Socket.IO, and a prerequisite graph of 69 topics (`graph/graph-data.json`) describes which topics depend on which.

## 3. Current state of the repository

- The project is active, with many open issues and several contributors working in parallel.
- `README.md` is now accurate on counts: 93 routes, 69 graph topics and 101 tiles.
- The server has 11 test files. The client got its test runner (Vitest) recently and has 7 test files so far.
- The big client file, `client/src/App.jsx` (about 70,900 lines), is being broken up. The `client/src/apps/` folder was created and the first component was extracted (#235).
- Browser history now works for mode changes (#245).
- Client lint had over 1,000 errors when issue #274 was written, and cleanup PRs are still landing, so the lint check is not blocking yet.

## 4. Gaps observed in the code

- **`client/src/App.jsx` is a 70,900-line file.** Almost every feature touches it, so PRs conflict often. CONTRIBUTING says testing the whole file is out of scope, so most of the app has no tests.
- **`server/index.js` is 3,160 lines** even though routes are split into 19 modules, so it still holds a lot of logic that is hard to review and test.
- **`/concept-api` looks like an orphan.** It is mounted at `server/index.js:626`, but nothing in `client/src` calls it. This is unused code that still has to be maintained and secured.
- **The Vachana lab's "Back to Home" button does not return to the home screen.** I saw this in the browser. The button is in `client/src/vachana/index.jsx` (around line 272) and calls `onBack`, which `App.jsx` provides. A student can get stuck in the lab.
- **The Mensuration Lab screen is headed "Mensuration"** even though its tile is named "Mensuration Lab". `MensurationLabApp` in `client/src/App.jsx` (around line 70787) sets `title="Mensuration"`, so it looks the same as the quiz.

## 5. Ideas for the project

- **Fix Vachana's Back button.**
  - *What:* make "Back to Home" return to the home grid.
  - *Why:* students get stuck in the lab.
  - *How:* trace how `onBack` is passed from `App.jsx` to the Vachana component and find where it stops working.
- **Rename the Mensuration Lab heading to match its tile.**
  - *What:* change the title shown inside the lab.
  - *Why:* two similar names confuse students.
  - *How:* a one-line change to the title in `MensurationLabApp`.
- **Add a check that the README counts match the code.**
  - *What:* a small script that counts the route prefixes and tiles and compares them with the README.
  - *Why:* the counts were wrong in the README because nothing checked them.
  - *How:* run the script in CI so a PR fails when the numbers drift.

## 6. My contribution

- PR #434: fixed the README's contradictory puzzle counts and the endpoint table (issue #231).
- PR #435: added the Mensuration Lab tile under Shape & Space (issue #217).
- PR #436: added the Vachana home-grid tile and removed duplicated entries from the hamburger menu (issue #212).

Before these, my dog mascot PRs (#162 and #354) were closed because of merge conflicts and no linked issue. I learned to claim the issue first, keep each PR small, and sync with `main` before pushing.