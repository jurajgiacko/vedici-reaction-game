# Firebase Setup Instructions

## Firestore Security Rules

Pre správne fungovanie aplikácie musíte nastaviť Firestore Security Rules v Firebase Console.

### Krok 1: Otvorte Firebase Console
1. Choďte na [Firebase Console](https://console.firebase.google.com)
2. Vyberte projekt `vedici-reaction-game`
3. V ľavom menu kliknite na **Firestore Database**
4. Prejdite na záložku **Rules**

### Krok 2: Nastavte Security Rules

Skopírujte a vložte tieto pravidlá:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Leaderboard collection
    match /leaderboard/{document} {
      // Allow anyone to read leaderboard
      allow read: if true;
      
      // Allow anyone to create new entries
      allow create: if request.resource.data.keys().hasAll(['email', 'score', 'timestamp', 'date'])
                    && request.resource.data.email is string
                    && request.resource.data.score is int
                    && request.resource.data.email.matches('.*@.*\\..*')
                    && request.resource.data.score > 0
                    && request.resource.data.score < 10000;
      
      // Prevent updates and deletes (optional - for security)
      allow update, delete: if false;
    }
  }
}
```

### Krok 3: Vytvorte Index (ak je potrebné)

Ak sa zobrazí chyba o chýbajúcom indexe:

1. V Firebase Console → Firestore Database → Indexes
2. Kliknite na link v chybovej správe alebo vytvorte index manuálne:
   - Collection: `leaderboard`
   - Fields: `score` (Ascending)
   - Query scope: Collection

### Krok 4: Publikujte Rules

1. Kliknite na **Publish** tlačidlo
2. Počkajte na potvrdenie

## Testovanie

Po nastavení pravidiel by mali fungovať:
- ✅ Načítanie leaderboardu
- ✅ Ukladanie nových skóre

## Riešenie problémov

### Chyba: "Permission denied"
- Skontrolujte, či sú security rules publikované
- Skontrolujte, či máte správne nastavené pravidlá

### Chyba: "Index required"
- Vytvorte index podľa inštrukcií vyššie
- Počkajte na dokončenie vytvárania indexu (môže trvať niekoľko minút)

### Chyba: "Firebase not initialized"
- Skontrolujte, či sú Firebase SDK správne načítané
- Skontrolujte konzolu prehlíadača pre chyby

