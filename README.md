# QiQi Studio Book

The private studio book and client loyalty cards for **QiQi Nail Studio** (@qiqi.nailstudio).

- **Studio book:** `https://ash1eigh9.github.io/qiqi-studio/` (sign in with your Firebase email and password)
- **Client card:** `https://ash1eigh9.github.io/qiqi-studio/card.html?c=<card code>` (each client gets their own link from their profile)

## What's in it

| File | What it is |
|---|---|
| `index.html` | Studio book: bookings, clients, press-ons, supplies, money, loyalty chops, promos, backup |
| `card.html` | Public loyalty card page clients open from their link |
| `config.js` | Your Firebase project settings (public identifiers, not passwords) |
| `manifest.webmanifest`, `icon*` | Lets you "Add to Home Screen" so it opens like an app |

## Where the data lives

Everything is stored in **Firebase Firestore** (free Spark plan):

- `studio/data/...`: your bookings, clients, press-ons, supplies and expenses. Only your account can read or change them.
- `cards/<code>`: one small document per client card (first name and chop count), readable only by someone who has that exact link.

The code in this repository holds no client data.

## Firestore rules

Paste these in Firebase → Firestore Database → Rules, with your own User UID:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    function isOwner() {
      return request.auth != null && request.auth.uid == "PASTE_YOUR_UID_HERE";
    }
    match /studio/{document=**} {
      allow read, write: if isOwner();
    }
    match /cards/{cardId} {
      allow get: if true;
      allow list: if false;
      allow write: if isOwner();
    }
  }
}
```

## Backups

Open the **Money** tab and use **Download backup** now and then. **Load backup file** restores from that file.
