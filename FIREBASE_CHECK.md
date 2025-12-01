# Firebase Setup Checklist

## ✅ Konfigurácia

Aktuálna Firebase konfigurácia v `index.html`:
- ✅ apiKey: `AIzaSyCvMuK_cxGhu-oCIBoYedbN0nB3QkJ5ghQ`
- ✅ authDomain: `vedici-reaction-game.firebaseapp.com`
- ✅ projectId: `vedici-reaction-game`
- ✅ storageBucket: `vedici-reaction-game.firebasestorage.app`
- ✅ messagingSenderId: `368959111382`
- ✅ appId: `1:368959111382:web:d1f7ef0043e223a49cc977`

## ✅ SDK Načítanie

Aktuálne používame **Firebase Compat SDK** (v9.0.0):
```html
<script src="https://www.gstatic.com/firebasejs/9.0.0/firebase-app-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/9.0.0/firebase-firestore-compat.js"></script>
```

Toto je správne pre jednoduché HTML súbory bez build procesu.

## 🔧 Kontrola Firebase Console

### 1. Firestore Database
- [ ] Firestore je **zapnuté** (Production mode alebo Test mode)
- [ ] Kolekcia `leaderboard` existuje (alebo sa vytvorí automaticky pri prvom zápise)

### 2. Security Rules
Otvorte: Firebase Console → Firestore Database → Rules

**Požadované pravidlá:**
```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /leaderboard/{document} {
      allow read: if true;
      allow create: if request.resource.data.keys().hasAll(['email', 'score', 'timestamp', 'date'])
                    && request.resource.data.email is string
                    && request.resource.data.score is int
                    && request.resource.data.email.matches('.*@.*\\..*')
                    && request.resource.data.score > 0
                    && request.resource.data.score < 10000;
      allow update, delete: if false;
    }
  }
}
```

### 3. Indexes
Ak používate `orderBy('score', 'asc')`, môže byť potrebný index:
- [ ] Vytvorte index ak sa zobrazí chyba "Index required"
- [ ] Collection: `leaderboard`
- [ ] Field: `score` (Ascending)

### 4. Web App Configuration
- [ ] Skontrolujte, či je web app registrovaný v Firebase Console
- [ ] Cesta: Firebase Console → Project Settings → Your apps

## 🧪 Testovanie

### V prehliadači (F12 Console):

1. **Kontrola inicializácie:**
   ```javascript
   // Mala by sa zobraziť správa: "Firebase initialized successfully"
   ```

2. **Test zápisu:**
   ```javascript
   // Skúste uložiť skóre cez UI
   // V konzole by sa nemali zobraziť chyby
   ```

3. **Test čítania:**
   ```javascript
   // Leaderboard by sa mal načítať bez chýb
   ```

## 🐛 Riešenie problémov

### "Permission denied"
- Skontrolujte Security Rules
- Uistite sa, že sú pravidlá publikované
- Skontrolujte formát dát (email musí byť string, score musí byť int)

### "Index required"
- Vytvorte index v Firebase Console
- Počkajte na dokončenie (môže trvať niekoľko minút)

### "Firebase not initialized"
- Skontrolujte, či sú SDK skripty načítané
- Skontrolujte konzolu prehlíadača pre chyby
- Overte, či je apiKey správny

### "Failed to load leaderboard"
- Skontrolujte Security Rules (read musí byť povolené)
- Skontrolujte, či existujú dáta v kolekcii
- Skontrolujte index ak používate orderBy

## 📝 Firebase CLI (voliteľné)

Ak chcete používať Firebase CLI pre deployment alebo emulátory:

```bash
# Inštalácia
npm install -g firebase-tools

# Prihlásenie
firebase login

# Inicializácia projektu
firebase init

# Spustenie emulátorov (pre lokálne testovanie)
firebase emulators:start
```

**Poznámka:** Pre jednoduché statické HTML súbory na Vercel nie je Firebase CLI potrebné.




