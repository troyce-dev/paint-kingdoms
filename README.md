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
   - **Hide join box**: hides the whole top-left box (title, web address and class code) once everyone has joined, so the full battlefield shows. Click **Show join box** to bring it back.
   - **New class code**: a fresh code and map for the next period. The old class is deleted.

Teacher control is tied to the browser that created the class. If you change computers or clear your browser data, create a new class code.

## How the game works
- **Goal:** be the last castle standing. The teacher's screen shows a live Top 3, ranked by the percentage of the map each kingdom has painted.
- **Levels:** the teacher picks Level 1, 2 or 3 on the projector screen, and can change it any time. Level 1 uses easier questions ($50 each). Level 2 uses Medium ($75) and Hard ($100). Level 3 uses Hard ($100) and Challenge ($150).
- **Right and wrong answers:** correct answers pay cash, plus a $25 bonus for every 3 in a row. A wrong answer costs no money, but it damages the student's own castle like a Medium hit and locks the next question for 10 seconds. Seven wrong answers in a row would destroy a full-health castle.
- **Start:** everyone starts with $50 and no paint.
- **Shop:**
  - Paint ×5 for $40.
  - Wall for $100. While it stands, the castle is harder to hit (shots must land near the middle) and spreading paint can't seep in. Every blob the wall stops weakens it: 9 Small, 4 Medium, 3 Large or 2 Mega blobs knock it down.
  - Health +25% for $75, up to 100%.
  - Bigger cannon: level 2 for $150 (Large loads) and level 3 for $300 (Mega loads).
- **Aim and fire:** point at the table to turn your cannon. Hold the mouse button (or Space) to charge, then let go. A dot on the aim line shows where the blob will land. Every cannon can reach the whole map. Loads use 1 to 4 paint. Arrow keys fine-tune the aim. The ⟲ ⟳ buttons turn the table.
- **Spreading paint:** a blob that lands on your own paint spreads for 30 seconds, growing to 1.7 times its size. If it seeps into a rival castle, that counts as a hit. Blobs that land on bare board or on someone else's paint don't spread, so you reach a rival by building a trail of your own paint toward them.
- **Destroying a castle:** it takes 15 Small, 7 Medium, 4 Large or 2 Mega hits (direct or seeped), or any mix that adds up to the same damage. Each castle shows a health bar. A destroyed castle crumbles to the ground and disappears, and all the paint that kingdom laid down disappears with it. The player who lands the final hit gets $40 in plunder.
- **Getting back in:** a student without a castle can't fire or shop, but keeps answering questions. A new castle costs $500. After buying it, the student clicks an open plot on the map (away from other castles and the edge) to build it. Cannon upgrades and paint carry over.
- **Sound:** cannons, splats, hits, crumbling castles, and right and wrong answers all have sound effects. Each screen has a *Sound: on/off* button.
- **Questions:** 140 written SAT-style math items (42 easy, 46 medium, 40 hard, 12 challenge) plus 53 question types that generate new numbers every time, chosen at random. Each Chromebook keeps a history of every question it has shown, so a student doesn't see the same question twice on that Chromebook, even after rejoining or starting a new class code.
- **Teacher buttons:** *Double money*, *Level 1/2/3*, *Hide join box*, *Hide battle log*, *Start a new round*, *New class code*.

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
