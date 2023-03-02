## <logos-firebase/> 使用 Firebase Auth 保護您的 API <!-- Secure your API with Firebase Auth -->

- [創建一個 Firebase 項目](https://firebase.google.com/?hl=zh-cn) <!-- Create a Firebase Project -->
  - Turn on Google Auth and Passwordless Email Auth in your Firebase Console
  - Add Firebase Project Config to your `.env` file: 
```
PUBLIC_FIREBASE_CONFIG={"apiKey":"...","authDomain":"YOURPROJECTID.firebaseapp.com","databaseURL":"https://YOURPROJECTID.firebaseio.com","projectId":"YOURPROJECTID","storageBucket":"YOURPROJECTID.appspot.com","messagingSenderId":"...","appId":"...","measurementId":"..."}
```

<div mt-5 v-click>

- 使用 [SvelteFireTS](https://sveltefirets.vercel.app/) 添加多語言 Firebase Auth <!-- Use SvelteFireTS to add multilingual Firebase Auth -->
  - Install: `npm i -D sveltefirets`
  - Use the `FirebaseUiAuth` component for multilingual sign-in:
```svelte
<script lang="ts">
  import { FirebaseUiAuth } from 'sveltefirets';
</script>

<FirebaseUiAuth 
  languageCode="zh_tw" 
  signInWith={{ google: true, emailPasswordless: true }} />
```
</div>

<!-- 
1. Create a Firebase Project - there are many good guides already on how to do this. 
2. 
-->

---

## <logos-firebase/> 使用 Firebase Auth 保護您的 API <!-- Secure your API with Firebase Auth -->

- Pass token to backend
```svelte {2,5-6,11}
<script lang="ts">
  import { authState } from 'sveltefirets';
  // ...
  async function query_documentation() {
    if (!$authState) throw new Error('Not logged in.')
    const auth_token = await $authState.getIdToken();
    const response = await fetch('/api/query', {
      method: 'POST',
      body: JSON.stringify({ 
        query,
        auth_token,
      }),
      headers: { 'content-type': 'application/json' }
    });
    answer = await response.json();
  }
</script>

<!-- ...search input... -->
```

---

## <logos-firebase/> 使用 Firebase Auth 保護您的 API <!-- Secure your API with Firebase Auth -->

- Verify token before answering question
  - Install Firebase Admin: `npm i -D firebase-admin` and create a new service account key
  - Create a function to verify the token:

```ts {all|4-5|9-13,17|16-19}
import { initializeApp, getApps, cert, type ServiceAccount } from 'firebase-admin/app';
import { getAuth } from 'firebase-admin/auth';
import type { DecodedIdToken } from 'firebase-admin/lib/auth/token-verifier';
import { FIREBASE_SERVICE_ACCOUNT_CREDENTIALS } from '$env/static/private';
const SERVICE_ACCOUNT: ServiceAccount & { project_id: string } = JSON.parse(FIREBASE_SERVICE_ACCOUNT_CREDENTIALS); 

function initializeFirebase() {
  if (!getApps().length) {
    initializeApp({
      credential: cert(SERVICE_ACCOUNT),
      databaseURL: `https://${SERVICE_ACCOUNT.project_id}.firebaseio.com`
    });
  }
}

export async function decodeToken(token: string): Promise<DecodedIdToken> {
  initializeFirebase();
  return await getAuth().verifyIdToken(token);
}
```

---

## <logos-firebase/> 使用 Firebase Auth 保護您的 API <!-- Secure your API with Firebase Auth -->

Then you can use that function in your server endpoint you created earlier to ensure a user is authenticated. At this point you can control usage based on the user and your needs. 

```ts
// src/routes/api/query/+server.ts

export const POST: RequestHandler = async ({ request, fetch }) => {
  const { query, auth_token } = await request.json();
  const decodedToken = await decodeToken(auth_token);
  const user_id = decodedToken?.uid;
  if (!user_id) throw error(400, "Unauthorized usage");
  // ...
}
```