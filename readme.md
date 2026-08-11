# 📘 PROJECT README — SHOP (PRITAM999-Style Gaming Store)

> **Last updated:** 11 August 2026
> **Repo:** https://github.com/arisujod/Shop
> **Live:** https://shop.arisugpt.in
> ⚠️ **SECURITY NOTE:** Ye repo abhi PUBLIC hai — is README me project credentials hai. Private hosting par shift hote hi repo PRIVATE karna ya sensitive section hatana.

---

## 1. PROJECT OVERVIEW

- **Goal:** `pritam999.shop` (original site) ka design pasand aaya → usko scrape karke **apni khud ki working shop** banana — same design, lekin real login/register/dashboard/products/admin sab working.
- **Approach:** Static HTML/CSS/JS + **Firebase** (Auth + Firestore) + **Cloudflare** (DNS, SSL, WAF, Turnstile). **No SQL, no PHP, no Firebase Storage** (Spark plan me Storage nahi milta → images sirf URLs ke roop me).
- **Hosting:** GitHub Pages (free) + custom domain `shop.arisugpt.in`. Final me apni **private hosting** par shift hoga.

---

## 2. IMPORTANT LINKS

| Cheez | Link |
|-------|------|
| Live site | https://shop.arisugpt.in |
| GitHub repo | https://github.com/arisujod/Shop |
| GitHub Pages URL | https://arisujod.github.io/Shop/ |
| Original reference site | https://pritam999.shop |
| Firebase console | https://console.firebase.google.com → project **Shop** |
| Cloudflare dashboard | dash.cloudflare.com → zone **arisugpt.in** |

---

## 3. CREDENTIALS & PRIVATE DETAILS 🔐

### GitHub
- Username: `arisujod` | Email: `arisujod@gmail.com`

### Firebase (HAMARA apna project) ✅
- Project name: **Shop** | Project ID: `shop-c8a7e` | Project number: `818351125426`
- Support email: `a416373727@gmail.com`
- Web App ID: `1:818351125426:web:1a85570ee665cca716eaab`
- **firebaseConfig (code me use hone wala):**
```js
const firebaseConfig = {
  apiKey: "AIzaSyAMump-ghutkg0beTytun6vhiS7SrDl1MM",
  authDomain: "shop-c8a7e.firebaseapp.com",
  projectId: "shop-c8a7e",
  storageBucket: "shop-c8a7e.firebasestorage.app",
  messagingSenderId: "818351125426",
  appId: "1:818351125426:web:1a85570ee665cca716eaab"
};
```
- **Auth providers:** Email/Password ✅ + Google ✅
- **Authorized domains:** localhost, shop-c8a7e.firebaseapp.com, shop-c8a7e.web.app, `shop.arisugpt.in`, `arisujod.github.io`
- **Firestore:** location `asia-south1` (Mumbai), abhi **test mode** me (baad me rules tighten karne hai)
- **Storage:** SKIP kiya (Spark plan me upgrade mangta hai) → images = URLs

### Test accounts
- Google test account: `niranjankumarg246@gmail.com` (Niranjan Gupta)

### Cloudflare (zone: arisugpt.in, account id: 3567d58e91eab0a25)
| Record | Type | Value | Proxy |
|--------|------|-------|-------|
| @ | A | 176.57.184.224 (main private hosting IP) | Proxied |
| shop | CNAME | arisujod.github.io | **Proxied** (WAF rule ke liye) |
| proxyi | CNAME | i5hsv04b.up.railway.app | Proxied |
- **SSL mode:** Full (Flexible nahi — redirect loop banta hai)
- **WAF custom rule:** Hostname equals `shop.arisugpt.in` → **Managed Challenge** (pastebin jaisa full-page verification, sirf shop par)
- **Bot Fight Mode:** ON | Browser Integrity Check: ON
- **Turnstile widget:** name "Shop Login", hostname `shop.arisugpt.in`, mode **Invisible** → **nayi site-key** code me lagi hai (secret key sirf Cloudflare dashboard me hai, code me nahi)

### PURANI / SCRAPED cheezein (code se HATANI hai / hat gayi)
- ❌ Old Firebase config: `terminalx999-auth.firebaseapp.com` (apiKey `AIzaSyB2quu...YNkvb0`) — original site ka tha, GitHub secret alert isi ki wajah se aaya (`user_dashboard.php#L508`, commit `480968c6`)
- ❌ Old Turnstile site-key: `0x4AAAAAADMaQnxJ0m4m1gi5` (original site ki)
- ❌ `auth_google.php`, `verify_2fa.php`, `shield.js` — original ke references, hamare project me kabhi exist nahi kiye

---

## 4. TECH STACK

- **Frontend:** pure HTML/CSS/JS (`.php` extension wali scraped files → rename to `.html`)
- **Design CSS:** `assets/css/core.css` (self-hosted copy — original `pritam999.shop/assets/css/core.css` se li gayi, ab hamari apni permanent copy)
- **Auth:** Firebase Auth (email/password + Google redirect flow)
- **Database:** Cloud Firestore (`users` collection live; `products` collection pending)
- **Images:** external raw.githubusercontent URLs (rose999yt ke repos) — Storage skip kiya
- **Security:** Cloudflare WAF Managed Challenge + Turnstile + Bot Fight Mode

---

## 5. COMPLETE JOURNEY — STEP BY STEP (kya kiya, mistake, fix)

**Day 1 (6 Aug):** Scraped files ko GitHub par push kiya (`.php` names).
- ❌ **Mistake/Issue:** GitHub **secret scanning alert** — original site ki Google API key code me thi.
- ✅ **Fix plan:** purana config hatana (abhi user_dashboard me bacha hai) → alert resolve.

**Analysis:** Files `.php` thi lekin content pure HTML/CSS/JS (Linguist: "Hack 100%"). Source = **scrap** — backend/DB/referenced files missing.

**Hosting decision:** GitHub Pages = static only (PHP nahi chalta). Free plan me **private repo + Pages nahi** milta → repo abhi public.

**Renames:** `.php` → `.html` + internal links fix. Beech me kuch Pages deployments red ❌ dikhe — **normal** (rapid pushes), final green ✅.

**Custom domain:**
- ❌ **Mistake:** pehle attempt me `shop .arisugpt.in` (space) type hua → "not properly formatted" error.
- ✅ **Fix:** exact `shop.arisugpt.in` → CNAME `shop → arisujod.github.io` (pehle DNS-only, baad me proxied) → SSL **Full** → HTTPS ✅ → `CNAME` file auto-created.

**Cloudflare protection:**
- IUM (Under Attack Mode) free dashboard me Settings me nahi mila aur wo poore zone par lagta → ❌ rejected.
- ✅ **Fix:** WAF custom rule (hostname = shop) → Managed Challenge → sirf shop par full-page verification ✅.

**core.css:**
- ❌ Hotlink risk (original owner change kar sakta tha).
- ✅ **Fix:** `assets/css/core.css` repo me self-hosted create ki.

**shield.js:** Analysis kiya (DevTools/copy/right-click blocker + debugger loop) → ❌ add nahi kiya, saare tags remove kiye.

**Firebase setup:** Project Shop banaya → Auth (Email+Google) → authorized domains → Firestore asia-south1 test mode → Storage ❌ (upgrade mangta hai) → **skip**, images = URLs (future: alag GitHub repo raw URLs).

**Turnstile:** Original ki key hamare domain par fail hoti → ✅ apna widget "Shop Login" banaya → **Success!** working.

**Firebase integration code:**
- ❌ **Mistake (hamari side):** `signInWithPopup` mobile (Brave) par fail → `auth/popup-closed-by-user`.
- ✅ **Fix:** `signInWithRedirect` + `getRedirectResult` (full-page Google flow — mobile reliable).
- ❌ **Mistake (editing me):** kai baar ids missing (`id="email"`, `id="password"`), register me Email field gayab, password input gayab, `user_dashboard.html` galti se login-page ban gayi, kabhi purani local files dobara upload ho gayi.
- ❌ **Mistake (AI reading):** GitHub cached content dikha kai baar → confusion → **fix:** user ne files direct upload karni shuru ki.
- ℹ️ Google consent me `shop-c8a7e.firebaseapp.com` dikhta hai — **normal** (Firebase authDomain), safe; custom branding baad me custom OAuth client se.

---

## 6. CURRENT STATUS (abhi kahan hai)

| Item | Status |
|------|--------|
| Site live (shop.arisugpt.in + HTTPS + CF challenge) | ✅ |
| core.css self-hosted | ✅ |
| shield.js removed | ✅ |
| Turnstile (apni key) working | ✅ |
| login.html — Email+Password + naya Firebase config | ✅ (ids verify karke push) |
| register.html — fields + Turnstile | 🔧 password input + redirect-script push hona baaki |
| Google login — redirect flow code | 🔧 push + test baaki |
| user_dashboard.html | ❌ abhi login-jaisa placeholder; purana terminalx999 block hatana baaki; asli dashboard pending |
| GitHub secret alert | ❌ open jab tak terminalx999 config sab files se nahi hat'ta |
| Products | ❌ abhi hardcoded HTML me; Firestore pending |
| Purchase button | ✅ blank (kuch nahi karta) |

---

## 7. ROADMAP (aage ke plans)

1. **Push + test:** login/register redirect-flow → Firestore `users` me entry verify.
2. **user_dashboard.html asli version:** logged-in user ka username/email/photo (Firebase se), logout button.
3. **Products → Firestore `products` collection** + index/shop/product-details ko **dynamic** banana (`?category=`, `?id=`).
4. **Seed script:** neeche Section 8 wala poora inventory ek click me Firestore me import.
5. **admin.html panel:** product add/edit/delete, categories, price, stock, image-URL manage; admin = `users.role` ya admins list se protect.
6. **Purchases:** abhi blank; payment flow baad me.
7. **Security hardening:** Firestore rules (test mode hatana), GitHub alert resolve, key rotate optional.
8. **Final deploy:** private hosting; optional Cloudflare Pages (private repo); optional custom OAuth client (consent me shop.arisugpt.in dikhane ke liye).

---

## 8. PRODUCT INVENTORY BACKUP (Firestore seed data)

> Image bases: `[AAA]`=rose999yt/AAA, `[SHOP]`=rose999yt/SHOP-PHOTO, `[P999]`=rose999yt/PRITAM-999, `[PAY]`=rose999yt/payment-method-logo (raw.githubusercontent paths)

### PC PANEL
- TERMINAL X BASIC PC PANEL | ₹500 | SOLD OUT | id:1766485953433 | [P999] ChatGPT Image Dec 24, 2025, 03_06_24 PM.png
- TERMINAL X 999 AIM SILENT EXE | ₹100 | SOLD OUT | id:1766486867031 | [P999] ChatGPT Image Dec 27, 2025, 01_55_18 AM.png
- TERMINAL X 999 EMULATOR BYPASS | ₹450 | SOLD OUT | id:1766487129260 | [P999] ChatGPT Image Dec 27, 2025, 01_55_22 AM.png
- EXTERNAL PANEL | ₹200 | 37 | id:products_6a002831e8c526.21027445 | [P999] ChatGPT Image May 10, 2026, 12_14_37 PM.png
- FREE PANEL | ₹1 | SOLD OUT | id:products_6a002f51adf878.11477833 | [P999] fe522bd…/ChatGPT Image May 10, 2026, 12_36_49 PM.png
- BR MOD AIMSILENT EXE | ₹90 | SOLD OUT | id:products_6a3d7da29e92a2.98730759 | [AAA] file_0000000022b47206bb7ac83712288887.png
- TERMINAL X 999 VAULT | ₹70 | SOLD OUT | id:products_6a439646768800.35036774 | [AAA] file_0000000006187206bbc6438a95dec391.png
- TERMINAL X 999 GROUP BAN TOOL | ₹800 | 97 | id:products_6a4442a6eb11c7.52412706 | [AAA] ChatGPT Image Jul 1, 2026, 03_49_10 AM.png

### ANDROID PANEL
- HG CHEAT MOD MENU / INJECTOR | ₹90 | 136 | id:1766528334662 | [SHOP] Picsart_26-02-27_22-08-21-497.jpg
- DRIP CLIENT MOD MENU | ₹80 | 141 | id:1766529749470 | [SHOP] Picsart_26-01-23_18-59-44-329.png
- BR MOD INJECTOR | ₹80 | 353 | id:1766530271879 | [SHOP] Picsart_26-01-23_22-20-11-039.png
- 𝗛𝗔𝗫𝗫𝗖𝗞𝗘𝗥 𝗣𝗥𝗢 𝗜𝗡𝗝𝗘𝗖𝗧𝗢𝗥 | ₹550 | 23 | id:1766565966002 | [SHOP] Picsart_26-01-23_23-02-44-951.png
- DRIP CLIENT ROOT INJECTOR | ₹80 | 13 | id:1766565966007 | [SHOP] Picsart_26-01-24_00-15-16-329.png
- PATO TEAM ORANGE MOD MENU | ₹150 | 44 | id:1766565966010 | [SHOP] Picsart_26-02-27_23-08-35-346.png
- PATO TEAM BLUE MOD MENU | ₹150 | 46 | id:1766565966013 | [SHOP] Picsart_26-02-27_23-21-53-003.png
- STRICKS BR INJECTOR | ₹90 | 60 | id:1766565966015 | [SHOP] Picsart_26-02-27_23-35-43-637.png
- PRIME MOD MENU | ₹80 | 38 | id:products_6a1731b88cd886.76520638 | [AAA] Picsart_26-05-27_23-13-44-665.png
- FLUX CHEATS INJECTOR | ₹70 | 44 | id:products_6a270a0c83bde0.34310471 | [AAA] Picsart_26-06-08_23-52-27-464.png
- TERMINAL X 999 PHONE | ₹140 | SOLD OUT | id:products_6a2848b13cb6c8.66056754 | [AAA] Picsart_26-06-09_18-09-44-807.png
- DRIP CLIENT PROXY | ₹90 | 86 | id:products_6a2c8e0d816ad3.38372845 | [AAA] 1023afc…/Picsart_26-06-13_04-19-22-950.png
- HG CHEATS PROXY | ₹80 | SOLD OUT | id:products_6a3d58ce69f378.74093657 | [AAA] Picsart_26-06-25_21-50-33-679.png
- EZ TEAM 8 BALL POOL | ₹80 | 29 | id:products_6a3db5eea09e40.03258735 | [AAA] 082d5e9…/Picsart_26-06-26_03-52-59-179.png

### FREE FIRE ID
- FACEBOOK LEVEL 8 ID | ₹60 | 5 | id:1766565966001 | [P999] 1766617255577.jpg
- GOOGLE LEVEL 8 ID | ₹50 | 1 | id:1766565966003 | [P999] 1766617726687.jpg
- LEVEL UP SERVICE | ₹999,999 | SOLD OUT | id:1766565966006 | [P999] Picsart_25-12-27_03-06-49-005.png

### MYSTERY BOX
- Mystery Box | ₹5,000 | UNLIMITED | id:1766565966004 | [PAY] image_search_1776445243665.jpg

### 𝗶𝗢𝗦 𝗣𝗔𝗡𝗘𝗟
- E-SIGN CERTIFICATE | ₹500 | 2 | id:1766565966011 | [SHOP] Picsart_26-02-26_18-02-49-659.jpg
- iPHONE SAFE PANEL | ₹450 | 13 | id:1766565966012 | [SHOP] Picsart_26-02-26_16-21-16-592.png
- MIGUL PRO MONITE IOS PANEL | ₹400 | SOLD OUT | id:products_6a43b9c20a9450.59980640 | [AAA] a849c11…/Picsart_26-06-30_17-57-58-948.png
- MIGUL BASIC MONITE IOS PANEL | ₹350 | SOLD OUT | id:products_6a43ba92cb2a08.78169700 | [AAA] a849c11…/Picsart_26-06-30_18-00-56-640.png

### PC GAME PANEL
- COUNTER STRIKE 2 PANEL | ₹1,200 | SOLD OUT | id:products_69ffb6a28cad40.68244920 | [AAA] ChatGPT Image May 10, 2026, 04_01_17 AM.png
- VALORANT PANEL | ₹1,300 | SOLD OUT | id:products_69ffb6ed5e6302.63252000 | [AAA] ChatGPT Image May 10, 2026, 03_59_26 AM.png
- GRAND RP PANEL | ₹1,300 | SOLD OUT | id:products_6a0022f6c2f354.32997336 | [AAA] file_00000000f9947208b907ffb526763f30.png

---

## 9. NOTES FOR FUTURE

- Naye products/images ke liye: alag GitHub repo bana kar raw URLs use karna (Storage nahi hai).
- Firestore test mode ko production rules se replace karna.
- Original site ke assets ab hamari copy hai (`core.css`) — original owner ke changes se farak nahi padta.
- Video asset: `assets/video/video_1782830065.mp4` (repo me existing).