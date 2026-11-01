# WS Staff Hub: Setup Guide

Chat (channels, group chats, DMs) plus support tickets for WS Lawn Care.
Works on phones and Chromebooks. About 30 minutes, no terminal needed.

**What's in the folder**

| File | What it does |
|---|---|
| `index.html` | The whole app |
| `config.js` | Your Firebase settings (you edit this one) |
| `firestore.rules` | Security rules (you paste these into Firebase) |
| `manifest.json`, `sw.js`, `icon-*.png` | Lets phones install it like an app |

**Cost:** Firebase's free plan (Spark) covers 3 to 10 staff easily. Hosting on Netlify is free too, and a custom domain is optional.

---

## 1. Create the Firebase project
1. Go to <https://console.firebase.google.com> and sign in with your Google account.
2. **Create a project**, name it `ws-staff-hub`, and turn Google Analytics **off**.

## 2. Turn on logins
1. **Build > Authentication > Get started**.
2. Under **Sign-in method**, enable **Email/Password** and save.
3. Open the **Users** tab and **Add user** for Willie, William and Sandra (their email plus a temporary password). Staff can use "Forgot password?" on the login screen to set their own.

There is no public sign-up. Only people you add here can get in.

## 3. Create the database and paste the rules
1. **Build > Firestore Database > Create database**.
2. Choose **Production mode** and pick a US location (for example `nam5`).
3. Open the **Rules** tab, delete what's there, paste everything from `firestore.rules`, then click **Publish**.

## 4. Connect the app to your project
1. Click the gear icon, then **Project settings**.
2. Scroll to **Your apps**, click the web icon `</>`, name it `staff-hub`, and register it (skip Firebase Hosting).
3. Copy the `firebaseConfig` values it shows into `config.js`, replacing each `PASTE_...` placeholder.

## 5. Put the app online (Netlify Drop)
1. Unzip this folder (Chromebook Files app: right-click the zip > **Extract all**).
2. Go to <https://app.netlify.com/drop> and drag the whole folder onto the page.
3. You get a web address like `https://something-random.netlify.app`. Copy it.
4. Back in Firebase: **Authentication > Settings > Authorized domains > Add domain**, and paste that address without `https://`. (Logins won't work until you do this.)

*Optional white-label address:* in Netlify, **Domain management > Add a domain** and use something like `hub.wslawncareservicesllc.com`. Netlify tells you the DNS record to add. Add that same domain to Firebase's authorized domains.

To update the app later, drag the folder onto the same Netlify site's **Deploys** page.

## 6. Make Willie the admin
1. Open the app address and sign in as Willie once. This creates his profile.
2. In Firebase, **Firestore Database > Data > users**, open Willie's document and change `role` from `staff` to `admin`.
3. Refresh the app. It creates the channels (#general, #field, #it-help, #procedures, #ticket-feed) and the default ticket categories.
4. As admin, use the **Categories** button on the Tickets screen to set your own ticket categories whenever you're ready.

Have William and Sandra sign in once so they show up for DMs and ticket assignment.

## 7. Put it in SuiteDash
Add a custom page or HTML block and use:

```html
<iframe src="https://YOUR-ADDRESS" style="width:100%;height:85vh;border:0"
        allow="clipboard-write; notifications"></iframe>
```

Also add a plain link button to the same address that opens in a new tab. **On iPhones, Safari blocks sign-ins inside embedded frames**, so staff on phones should use the app directly (next step) rather than through SuiteDash. Chromebooks should work in either place.

## 8. Install on phones
- **Android (Chrome):** open the address, menu (three dots), **Install app**.
- **iPhone (Safari):** open the address, Share, **Add to Home Screen**.
- Tap **Turn on notifications** in the app's sidebar.

---

## How it works
- **Channels:** every staff member sees them. Add or rename them in the `CHANNELS` list near the top of the script in `index.html`.
- **Groups:** the **+** next to Groups lets anyone create a private group.
- **DMs:** the **+** next to Direct messages.
- **Tickets:** anyone can open one, assign it, change status and priority, and reply. New tickets and status changes also post automatically to #ticket-feed. All staff can see all tickets.
- **Unread:** a green dot marks unread conversations and the browser tab shows a count.
- **Weak signal:** the app keeps working offline and sends messages when the connection returns.

## Limits in this first version
- **Notifications only arrive while the app is open** (or running in the background on a phone). Alerts when the app is fully closed need a small add-on (Firebase push messaging and one cloud function; that needs the pay-as-you-go plan, still pennies at your size).
- **No photo or file attachments yet** (useful for field issues; Firebase Storage can add it).
- **Groups can't have members added later** after they're created.
- Firebase keys in `config.js` are meant to be public. Don't loosen `firestore.rules` or anyone could read your data.
