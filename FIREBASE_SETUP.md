# icare_qatar: Firebase setup (about 10 minutes, free plan)

1. Go to https://console.firebase.google.com and create a project (any name). Skip Google Analytics.
2. Build > Firestore Database > Create database > production mode > pick a region (e.g. europe-west or me-central).
3. Build > Authentication > Get started > Sign-in method > enable Email/Password.
   Then Users > Add user: your owner email and a strong password.
4. Firestore > Rules tab: replace everything with the rules below (use your owner email), then Publish.
5. Project settings (gear icon) > Your apps > Web (</>) > register an app > copy the firebaseConfig values.
6. Open index.html, find FIREBASE_CONFIG and ADMIN_EMAIL near the top of the script, paste your values, save.
7. Upload index.html to your host (Netlify Drop, Cloudflare Pages or GitHub Pages).
8. Authentication > Settings > Authorized domains: add your site's domain.
9. Open your site, click "Owner login" in the footer, sign in, and use "Manage stock".

## Firestore rules (everyone can read, only you can write)

rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /shop/{doc} {
      allow read: if true;
      allow write: if request.auth != null
                   && request.auth.token.email == "owner@example.com";
    }
  }
}
