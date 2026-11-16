# Paint Kingdoms

A 3D classroom math game for up to 25 players. Students answer SAT-style questions (mostly Algebra 2) to earn money, buy paint, shields and bigger cannons, and knock down each other's castles until one is left standing. No student sign-in: students go to the website and type a class code.

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
- only the teacher who created a class can start new rounds, turn on double money, or close the class,
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
   - **Turn on double money**: every correct answer pays double.
   - **Start a new round**: wipes the map. Students keep the code and rejoin.
   - **Hide class code**: hides the code on the projector once everyone has joined. Click again to show it.
   - **New class code**: a fresh code and map for the next period. The old class is deleted.

Teacher control is tied to the browser that created the class. If you change computers or clear your browser data, create a new class code.

## How the game works
- **Goal:** be the last castle standing. The teacher's screen also shows a live Top 3 leaderboard, ranked by castles destroyed.
- **Money:** every correct answer pays cash: $50 for Medium, $100 for Hard, $150 for Challenge questions, plus a $25 bonus for every 3 in a row. Every wrong answer costs $50 and locks the next question for 10 seconds. Everyone starts with $100 and 4 paint.
- **Shop:** Paint ×5 for $40. A shield for $100 that blocks the next 2 direct hits. A bigger cannon: level 2 for $150 (more range, Large loads) and level 3 for $300 (longest range, Mega loads).
- **Aim and fire:** point at the table to turn your cannon. Hold the mouse button (or Space) to charge, then let go. A dot on the aim line shows where the blob will land. Loads use 1 to 4 paint. Arrow keys fine-tune the aim. The ⟲ ⟳ buttons turn the table.
- **3 direct hits:** a castle that takes 3 direct hits crumbles to the ground and disappears, and all the paint that kingdom laid down disappears with it. A splat of the attacker's paint marks the spot. The player who lands the final hit gets $100 in plunder.
- **Debt and bankruptcy:** students can go into debt, but owing more than $150 makes their castle go bankrupt and crumble. Bankruptcy wipes the debt back to $0.
- **Getting back in:** a student without a castle can't fire or shop, but keeps answering questions. A new castle costs $750. After buying it, the student clicks an open plot on the map (away from other castles and the edge) to build it. Cannon upgrades and paint carry over.
- **Sound:** cannons, splats, hits, shields, crumbling castles, and right and wrong answers all have sound effects. Each screen has a *Sound: on/off* button.
- **Questions:** 80 written SAT-style math items plus 35 question types that generate new numbers every time, chosen at random. Each Chromebook remembers which questions it has seen, so students don't get repeats.
- **Teacher buttons:** *Double money* doubles every correct answer's payout.

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
