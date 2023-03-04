## <logos-firebase/> 使用 Firebase Auth 保護 API <!-- Secure your API with Firebase Auth -->

- [創建一個 Firebase 項目](https://firebase.google.com/?hl=zh-cn) <!-- Create a Firebase Project -->
  - 在 Firebase Console 打開 Google Auth 和 Passwordless Email Auth 
  - 把 Firebase Project Config 放在 `.env` 文件: 
```
PUBLIC_FIREBASE_CONFIG={"apiKey":"...","authDomain":"YOURPROJECTID.firebaseapp.com","databaseURL":"https://YOURPROJECTID.firebaseio.com","projectId":"YOURPROJECTID","storageBucket":"YOURPROJECTID.appspot.com","messagingSenderId":"...","appId":"...","measurementId":"..."}
```

<div mt-5 v-click>

- 使用 [SvelteFireTS](https://sveltefirets.vercel.app/) 添加多語言 Firebase Auth <!-- Use SvelteFireTS to add multilingual Firebase Auth -->
  - `npm install -D sveltefirets`
  - 使用 `FirebaseUiAuth` component 進行多語言登入：
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
... English is the default, add `zh_tw` for traditional Chinese 

layout: iframe-right
url: https://sveltefirets.vercel.app/1-auth/spanish
-->

---

## <logos-firebase/> 使用 Firebase Auth 保護 API <!-- Secure your API with Firebase Auth -->

在 API 請求中包含 auth token

```svelte {3,6-7,12}
<!-- src/routes/query/+page.svelte -->
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

<!-- Once a user is logged in you can get an auth token to and pass it to your backend along with the user's question -->


---

## <logos-firebase/> 使用 Firebase Auth 保護 API <!-- Secure your API with Firebase Auth -->

解碼 API 端點中的 auth token

```ts {all|5}
// src/routes/api/query/+server.ts

export const POST: RequestHandler = async ({ request, fetch }) => {
  const { query, auth_token } = await request.json();
  const decodedToken = await decodeToken(auth_token);
  const user_id = decodedToken?.uid;
  if (!user_id) throw error(400, "Unauthorized usage");
  // ...
}
```

<!-- 解碼 API 端點中的 auth token 以確保用戶已通過身份驗證。 At this point you can control usage based on the user and your needs. If they are not authenticated, throw an error.

Now we need to create that decodeToken function.-->

---

## <logos-firebase/> 使用 Firebase Auth 保護 API <!-- Secure your API with Firebase Auth -->

```ts {4-5|1-2,9-13,17|16-19}
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

<ul>
<li>
創建一個新的 firebase-admin service account key
</li>
<li v-if="$slidev.nav.clicks > 0">
安裝 firebase-admin: `npm install -D firebase-admin`</li>
<li v-if="$slidev.nav.clicks > 2">
<carbon-chat /> 把相關文檔部分和用戶的問題發送給 OpenAI <!-- Send relevant documentation and user's question to OpenAI -->
</li>
</ul>


<!-- Then verify and decode the auth token. 

That's all for the code walkthrough today:
- we processed our documentation
- add a front-end search input
- added a backend to answer questions
- and secured everything with Firebase Authentication

That will create a working tool, but there are some logical next steps we could take with a project like this... -->