# VEDICI Focus Challenge

Reaction speed test game with Firebase leaderboard integration.

## Features

- ⚡ Real-time reaction time testing
- 🏆 Firebase-powered leaderboard
- 📧 Email-based score submission
- 📱 Mobile-optimized interface
- 🎨 Modern, responsive design

## Firebase Setup

The game uses Firebase Firestore to store leaderboard data. Make sure your Firebase project is configured with:

- **Collection**: `leaderboard`
- **Fields**: `email`, `score`, `timestamp`, `date`

## Deployment to Vercel

### Option 1: Using Vercel CLI

1. Install Vercel CLI:
   ```bash
   npm i -g vercel
   ```

2. Deploy:
   ```bash
   vercel
   ```

3. Follow the prompts to link your project

### Option 2: Using GitHub Integration

1. Push your code to GitHub (already done)
2. Go to [vercel.com](https://vercel.com)
3. Import your GitHub repository
4. Vercel will automatically detect the configuration

### Custom Domain Setup

1. In Vercel dashboard, go to your project
2. Navigate to **Settings** → **Domains**
3. Add your custom domain
4. Follow DNS configuration instructions
5. Vercel will automatically provision SSL certificate

## Project Structure

```
.
├── index.html          # Main game file
├── vercel.json         # Vercel configuration
├── .gitignore          # Git ignore rules
└── README.md           # This file
```

## Firebase Configuration

The Firebase config is already included in `index.html`. Make sure your Firebase project has:

- Firestore Database enabled
- Security rules allowing writes to `leaderboard` collection

Example Firestore security rules:
```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /leaderboard/{document} {
      allow read: if true;
      allow create: if request.resource.data.keys().hasAll(['email', 'score', 'timestamp']);
    }
  }
}
```

## License

© VEDICI




