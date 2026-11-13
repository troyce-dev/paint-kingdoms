# Paint Kingdoms

A 3D classroom math game for up to 25 players. Students answer SAT-style questions (mostly Algebra 2) to earn paint, aim cannons to fire it, and swamp each other's castles until one empire rules the map. No student sign-in: students go to the website and type a class code.

- **Student page:** `https://troyce-dev.github.io/paint-kingdoms/`
- **Teacher / projector page:** `https://troyce-dev.github.io/paint-kingdoms/#teacher`

The site is hosted free on GitHub Pages. Live multiplayer runs on a free Google Firebase database. Students are connected with anonymous Firebase accounts, so they never type an email or password.

Until step 4 is done, the site runs in **practice mode**: everything works for one player, but there is no shared class map.

---

## One-time setup (about 15 minutes)

### 1. Create a Firebase project
1. Go to <https://console.firebase.google.com> and sign in with a Google account.
2. Click **Create a project** (or **Add project**). Name it `paint-kingdoms`.
3. You can turn **Google Analytics** off. Click **Create project**.

### 2. Turn on anonymous sign-in
1. In the left menu, open **Build → Authentication** and click **Get started**.
2. On the **Sign-in method** tab, choose **Anonymous**, switch it **on**, and click **Save**.
3. On the **Settings** tab, open **Authorized domains**, click **Add domain**, and add `troyce-dev.github.io`.

### 3. Create the database and lock it down
1. Open **Build → Realtime Database** and click **Create Database**.
2. Pick the location closest to you (for example, United States). Choose **Start in locked mode**. Click **Enable**.
3. Open the **Rules** tab. Delete everything in the box, paste the full contents of [`database.rules.json`](database.rules.json), and click **Publish**.

These rules make sure:
- students can only change their own kingdom,
- only the teacher who created a class can start new rounds, turn on double paint, or close the class,
- nobody can browse the list of classes; you need the code.

### 4. Connect the website to your project
1. Click the gear icon next to **Project Overview** → **Project settings**.
2. Under **Your apps**, click the web icon **`</>`**. Name the app `Paint Kingdoms`. Leave "Firebase Hosting" unchecked. Click **Register app**.
3. Firebase shows a block of code with `const firebaseConfig = { ... }`. Copy the part between the curly braces.
4. On GitHub, open [`firebase-config.js`](firebase-config.js) and click the pencil icon to edit it. Replace the placeholder values with yours, keeping the `window.FIREBASE_CONFIG = { ... };` wrapper. Commit the change.
5. Make sure your config includes a `databaseURL` line. If it doesn't, copy the URL shown at the top of the **Realtime Database → Data** tab (it ends in `firebaseio.com` or `firebasedatabase.app`).

These values are safe to publish. They name your project. The rules from step 3 are what protect it.

### 5. Turn on GitHub Pages
1. In this repository, open **Settings → Pages**.
2. Under **Build and deployment**, set **Source** to **Deploy from a branch**. Choose branch **main** and folder **/ (root)**. Click **Save**.
3. After about a minute the site is live at `https://troyce-dev.github.io/paint-kingdoms/`.

### 6. Check it
Open the student page. The pill at the top should say **Live**, not "Practice mode". If it still says practice mode, re-check `firebase-config.js` (step 4).

---

## Running a class

1. On the projector computer, open `https://troyce-dev.github.io/paint-kingdoms/#teacher` and click **Create class code**.
2. Students go to `troyce-dev.github.io/paint-kingdoms` on their Chromebooks, type the code, choose a kingdom name and one of 25 colors, and start answering.
3. Teacher buttons on the projector:
   - **Turn on double paint**: every correct answer is worth twice the paint.
   - **Start a new round**: wipes the map. Students keep the code and rejoin.
   - **New class code**: a fresh code and map for the next period. The old class is deleted.

Teacher control is tied to the browser that created the class. If you change computers or clear your browser data, create a new class code.

## How the game works
- **Goal:** conquer every kingdom. The game is won when one empire owns every castle on the map.
- **Aim and fire:** point at the table to turn your cannon. Hold the mouse button (or Space) to charge, then let go. The power sets how far the blob flies (1.5 to 15 away), and a dot on the aim line shows where it will land. Choose a Small (1 paint), Medium (2) or Large (3) load. Arrow keys fine-tune the aim. The ⟲ ⟳ buttons turn the table.
- **Swamping:** every castle has a ring around it. Cover 70% of that ring with your empire's paint to capture the castle. Capture a kingdom's *home* castle and its whole empire, every castle it owns, joins you.
- **Conquered players keep playing.** They fight for the empire that captured them, their shots paint in that empire's color, and they win if it wins.
- **Correct answer:** 3 paint (medium) or 5 paint (hard), +2 for every 3 in a row, and a fresh coat of your color splashed on your own castle.
- **Wrong answer:** your shield drops for 45 seconds (your castle can be captured at 45% instead of 70%), 3 of your blobs explode off the board, and the next question is locked for 10 seconds.
- **Questions:** 42 written SAT-style items plus 23 question types that generate new numbers every time. Each Chromebook remembers which questions it has seen, so students don't get repeats.

## School network notes
If the game won't load on student Chromebooks, ask IT to allow:
- `troyce-dev.github.io`
- `www.gstatic.com` (Firebase code)
- `*.firebaseio.com` and `*.firebasedatabase.app` (game data)
- `identitytoolkit.googleapis.com` and `securetoken.googleapis.com` (anonymous sign-in)
- `fonts.googleapis.com` and `fonts.gstatic.com` (fonts; optional)

## Privacy
- Students don't create accounts or enter emails.
- The only thing a student types is a kingdom name. Ask students not to use their full real names.
- Game data stays in your Firebase project. Starting a new class code deletes the old class.

## Free-plan limits
Firebase's free Spark plan allows 100 simultaneous connections and 1 GB of stored data. That is well above what one class of 25 uses.

## Files
| File | What it is |
|---|---|
| `index.html` | The whole game |
| `firebase-config.js` | Your Firebase project settings (edit this once) |
| `database.rules.json` | Security rules to paste into Firebase |
| `three.min.js` | The 3D engine (three.js r128, MIT license, see `THREE-LICENSE.txt`), included so school filters don't block it |
