# Install the dashboard as a real TV app (Tizen Developer Mode)

Goal: the dashboard as an **installed app** on the Samsung U8000F — an icon in
Apps / on the Home bar, fullscreen, no browser chrome. This is the closest
Tizen equivalent of the CleanYourTV launcher install (Android TVs get a
launcher APK via ADB; Tizen has no ADB and no third-party Home takeover —
confirmed in the original project's own Samsung recon:
`docs/devices/samsung-tizen.md` in Gathin23/cleanyourtv).

The app package lives in [`../tizen-app/`](../tizen-app/) (`index.html` +
`config.xml` + `icon.png`). It becomes a `.wgt` once signed.

## Why the extra steps exist

Samsung only lets a TV install non-store apps in **Developer Mode**, and the
package must be signed with a **Samsung certificate** whose distributor
certificate contains **this TV's DUID**. The signing and the install must run
from a computer **on the same Wi-Fi as the TV**.

## A. On the TV (~2 minutes)

1. Home → **Apps** panel.
2. Open **App Settings** (or Apps → Settings gear), then press **1 2 3 4 5**
   on the remote (use the 123 button / on-screen numpad) → Developer Mode
   dialog appears.
3. **Developer Mode: On**. In **Host PC IP**, enter the IP address of the
   home computer that will do the install.
4. Restart the TV. "Developer Mode" should now be written at the top of the
   Apps screen.

## B. On the home computer (one-time setup, ~20–30 min)

1. Install **Tizen Studio** (developer.tizen.org) with, via its Package
   Manager: **TV Extensions**, **Samsung Certificate Extension**,
   **Certificate Manager**, **Web CLI**.
2. **Tools → Device Manager → Remote Device Manager** → add the TV
   (its IP, port **26101**) → switch **Connection** ON. The TV is now
   reachable; right-click it → **DUID** and note the value.
3. **Tools → Certificate Manager** → **+** → **Samsung** → device type **TV**
   → create a certificate profile (new author certificate). It will ask you
   to **sign in with a Samsung account** in the browser (free). Add the TV's
   DUID when it asks for the distributor certificate's device list.
4. Right-click the TV in Device Manager → **Permit to install applications**.

## C. Build, sign, install (~2 minutes, and for every update)

From this repo:

```
cd tizen-app
tizen build-web -e .gitignore -e README.md -- .   # or open in Tizen Studio
tizen package -t wgt -s <your-cert-profile-name> -- .buildResult/*.wgt
tizen install -n <signed>.wgt -t <TV name>
```

Or in Tizen Studio: import the `tizen-app` folder as a Tizen web project →
right-click → **Run As → Tizen Web Application** (builds, signs, installs,
launches).

## D. Make it permanent on the TV

1. Find **Kunmi Home** in Apps → select it → **Add to Home** (pins it to
   the Home bar like any store app).
2. Keep **Autorun Last App ON** (All Settings → General & Privacy →
   Start Screen Options) and the keep-awake settings from the cleanup guide,
   so it reopens fullscreen on power-on after last use.

## Honest limits & care notes

- It is an installed app, not a Home replacement: Samsung does not let any
  third-party app take over the Home button on Tizen.
- Developer-mode installs are for testing: a major Samsung firmware update
  can remove the app (re-run part C), and Samsung certificates expire
  (re-sign with a fresh profile if installs/launches start failing).
- Developer Mode connects the TV to ONE host PC IP. Changing the host means
  re-entering the IP in step A3.
- Turning Developer Mode off again: same 12345 dialog → Off → restart.
