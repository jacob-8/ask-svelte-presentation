## <logos-firebase/> 使用 Firebase Auth 保護 API <!-- Secure your API with Firebase Auth -->

<div v-click>

- [創建一個 Firebase 項目](https://firebase.google.com/?hl=zh-cn) <!-- Create a Firebase Project -->
  - 在 Firebase Console 打開 Google Auth 和 Passwordless Email Auth 
  - 把 Firebase Project Config 放在 `.env` 文件: 
```
PUBLIC_FIREBASE_CONFIG={"apiKey":"...","authDomain":"YOURPROJECTID.firebaseapp.com","databaseURL":"https://YOURPROJECTID.firebaseio.com","projectId":"YOURPROJECTID","storageBucket":"YOURPROJECTID.appspot.com","messagingSenderId":"...","appId":"...","measurementId":"..."}
```

</div>

<div mt-5 v-click>

- 使用 [SvelteFireTS](https://sveltefirets.vercel.app/) 添加多語言 Firebase Auth <!-- Use SvelteFireTS to add multilingual Firebase Auth -->
  - `npm install -D sveltefirets`
  - 使用 `FirebaseUiAuth` component 進行多語言登入 (38語言)：
```svelte
<script lang="ts">
  import { FirebaseUiAuth } from 'sveltefirets';
</script>

<FirebaseUiAuth 
  languageCode="zh_tw" 
  signInWith={{ google: true, emailPasswordless: true }} />
```

(`FirebaseUiAuth` component 利用 [FirebaseUI for Web](https://github.com/firebase/firebaseui-web))
</div>

<!-- 
- 首先創建一個 Firebase 項目, 很簡單，網上有很多教程。

Read all bullets

- 默認(mòrèn)是英文，為繁體中文加 `zh_tw` 
-->

---

## <logos-firebase/> 使用 Firebase Auth 保護 API <!-- Secure your API with Firebase Auth -->

在 API 請求中包含 `auth_token`

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

<!-- bāohán

使用者登入(1-4)後，你可以在前端獲得一個 auth token，並把它和使用者的問題一起傳遞(chuándì)到你的後端。

After a user is logged in you can get an auth token in the frontend and pass it to your backend along with the user's question. -->


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

<!-- 為確保使用者已通過(tōngguò)身份驗證(shēnfèn yànzhèng), 在 API 端點中我們需要把 auth token 解碼 (3-3). 如果他未通過身份驗證，拋(pāo)出錯誤。

(click) 

然後我們需要編寫(1-3) decodeToken 函數。

To make sure the user is authenticated, we need to decode the auth token in the API endpoint. If they are not authenticated, throw an error.

Now we need to create that decodeToken function.
-->

---

## <logos-firebase/> 使用 Firebase Auth 保護 API <!-- Secure your API with Firebase Auth -->

```ts {5-6|2-3,10-14,18|17-20}
// src/lib/server/firebase-admin.ts
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
<carbon-chat /> 把相關文檔部分和使用者的問題發送給 OpenAI <!-- Send relevant documentation and user's question to OpenAI -->
</li>
</ul>


<!-- 
Read 2 bullets

- 初始化 firebase-admin (click)
- 驗證和解碼 auth token

這就是今天的代碼介紹的全部內容:
- 我們處理了我們的文件
- 添加了前端搜索輸入
- 添加了一個後端來回答問題
- 最後使用 Firebase Auth 保護 API

English:
- Initialize firebase-admin
- Verify and decode the auth token

That's all for the code walkthrough today:
- we processed our documentation
- added a frontend search input
- added a backend to answer questions
- and secured everything with Firebase Authentication
 
-->