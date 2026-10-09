# OPEN OUTCRY

OPEN OUTCRY is a browser-based, role-driven trading-floor simulation. The application is implemented in plain HTML, CSS, and JavaScript, served and bundled with Vite. It does not use React or TypeScript.

## Trading stations

- **Exchange Operator:** pause or resume the market, configure simulation speed from 0.5× to 4× for the shared round countdown and market movement, halt an instrument, apply demo price shocks, publish exchange headlines, and advance the round.
- **Prime Broker Desk:** review client orders, execute or reject them, and inspect simulated client exposure.
- **Floor Trader Terminal:** browse a searchable catalog of 582 simulated instruments across stocks, IPOs, bonds, ETFs, commodities, currencies, crypto currencies, CEX pairs, and DEX pairs. The selected instrument has a selectable 15-second, 1-minute, or 5-minute OHLC candlestick chart. Broker-executed buys/sells drive simulated order flow, candle prices, and volume.
- **Pit Overview Board:** display exchange prices, floor standings, and headlines in read-only mode.
- **VIP Spectator Gallery:** monitor the market and leaderboard and cast local demo poll votes.

## Multiplayer rooms

Sign in with Google, open **Game room**, then create a room or join using an invite code. In an active room, use **Copy Invite Link** to copy a URL containing the room code. Opening that link prompts the invited player to sign in and join the room. Rooms support up to 30 authenticated participants.

Firebase Authentication identifies room members. The **Sign in with Google** button uses Firebase Google Authentication, falls back to the redirect flow when browser popups are blocked, and restores the session after returning to the app. Firestore stores the room roster and synchronizes market data, game speed, orders, trader positions, headlines, and spectator polls in real time. The room host advances the shared game clock; the active room is remembered in the browser so a signed-in participant can reconnect after a reload.

To enable sign-in, enable the Google provider under Firebase Console → Authentication → Sign-in method and add the app's hostname under Authentication → Settings → Authorized domains. Add only the hostname, without `https://` or a port (for example, `localhost`, `127.0.0.1`, or your deployed domain). The exact hostname in the browser address bar must be authorized. Sign-in errors identify the Firebase setup step that needs attention.

When no multiplayer room is joined, market state is simulated locally and stored in browser local storage. The catalog includes 576 generated instruments plus six featured instruments. Its filters and search help locate a symbol by category, name, or sector. The chart retains bounded candle history and shows open, high, low, close, volume, and simulated buyer/seller pressure. Broker-executed orders influence price movement; generated background flow adds small demo-market variation. All listings, prices, and fills are fictional demo data, not real market activity or live financial data.

## Getting started

Prerequisites: Node.js 18+ and npm.

```bash
npm install
npm run dev
```

Open the local Vite URL (by default, `http://localhost:3000`). Do not open `index.html` directly from the filesystem: Firebase's npm modules are bundled by Vite.

## Checks

```bash
npm run build
npm run lint
```

## Publish with Firebase Hosting

The Hosting config publishes the Vite production build from `dist/`, applies cache/security headers, and routes app paths back to the single-page app entry. The default Firebase project is selected in `.firebaserc`.

1. Install the Firebase CLI if it is not already available: `npm install --global firebase-tools`.
2. Sign in and confirm access to the configured project: `firebase login` and `firebase projects:list`.
3. Build and deploy Hosting: `npm run deploy`.

After deployment, add the deployed `web.app` hostname and any custom domain to Firebase Console → Authentication → Settings → Authorized domains, and confirm Google is enabled under Authentication → Sign-in method. Browser-based Firebase configuration is public by design; Firestore access must remain protected by the deployed Firestore security rules. This demo is not a production financial service.

## Firebase configuration

Firebase settings are read from `firebase-applet-config.json`. To use another Firebase project, replace the settings with that project's web app configuration and configure Google sign-in and authorized domains in Firebase Authentication. OAuth provider credentials and authorized domains are managed in the Firebase Console; they cannot be enabled from the client app.

## Source layout

```text
index.html              HTML document and app mount
src/vanilla.css         Plain CSS for the gateway and role terminals
src/vanilla-main.js     Vanilla JavaScript application and Firebase integration
vite.config.js          JavaScript Vite configuration
standalone.html         Self-contained HTML/CSS/JavaScript demo page
firebase-applet-config.json
firestore.rules         Firebase access rules
```

This is a demo, not a production trading or execution system. Quotes, fills, polls, and role actions are simulated. Room access relies on Firebase Authentication and the rules in `firestore.rules`; role selection and simulated trading actions are client-side game mechanics, not an authorization boundary for financial systems.
