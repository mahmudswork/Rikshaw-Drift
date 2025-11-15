Rickshaw Drift — Host-ready package (Advanced)
=================================================

Files included:
- index.html                -> Main page (leaderboard + game container)
- phaser_index.html         -> Simple Phaser demo using assets/
- assets/                   -> placeholder assets and firebase helper

What you must change before hosting:
1) Replace Firebase config in index.html with your project's config (apiKey, authDomain, projectId, storageBucket, messagingSenderId, appId).
2) (Optional) Set your Discord webhook URL in the postDiscord() function inside index.html if you want Discord notifications.
3) Replace placeholder images (assets/*.png) with your polished sprites (rickshaw.png, bus.png, coin.png, logo_placeholder.png).
4) Replace placeholder audio (assets/*.wav) with real music/SFX files for production.
5) Set Firestore rules (see README notes below).

Deploy:
- Push the entire folder to a GitHub repository and enable GitHub Pages, or deploy on Netlify / Vercel.

Firestore security rules (recommended):
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /leaderboard/{docId} {
      allow read: if true;
      allow write: if request.resource.data.score is number
                   && request.resource.data.name is string
                   && request.resource.data.name.size() <= 24;
    }
    match /rooms/{roomId}/leaderboard/{docId} {
      allow read: if true;
      allow write: if request.resource.data.score is number
                   && request.resource.data.name is string
                   && request.resource.data.name.size() <= 24;
    }
  }
}

How to submit a score from your game:
- Ensure index.html is loaded on the same site (submitScore is exposed on window).
- Call: window.submitScore(playerName, score);

