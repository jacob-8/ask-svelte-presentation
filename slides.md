---
theme: seriph
background: https://source.unsplash.com/fVBWN3_ST0E/1920x1080 # selected from a curated Unsplash collection by Anthony https://unsplash.com/collections/94734566/slidev
class: 'text-center'
highlighter: shiki
lineNumbers: true
info: |
  ## Ask Svelte Presentation for March 2023 TOOCON
drawings:
  persist: false
# transition: fade-out
css: unocss
---

# 問 Svelte

使用 AI 以任何語言搜索知識庫

<div class="pt-12">
  <span @click="$slidev.nav.next" class="px-2 py-1 rounded cursor-pointer" hover="bg-white bg-opacity-10">
    按空格鍵下一頁
    <!-- Press Space for next page  -->
    <carbon:arrow-right class="inline"/>
  </span>
</div>

<div class="abs-br m-6 flex gap-2">
  <a href="https://github.com/jacob-8/ask-svelte-presentation" target="_blank" title="GitHub"
    class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-logo-github />
  </a>
</div>

<!--
了解我如何使用 OpenAI 和 SvelteKit 為 Svelte 文檔站點構建類似 ChatGPT 的多語言對話搜索。
了解我如何使用 Firebase Auth 保護我的 API，並獲得一些關於如何使用 Firebase 為您自己或您的公司創建多語言搜索工具的想法。
-->

---

# 搜索演示

<img m="y-10" border="rounded" src="/favorite-zh.png">

[polylingual.dev/zh-TW/search](https://polylingual.dev/zh-TW/search)

---
layout: iframe-right
url: https://polylingual.dev
---

# 搜索演示 <!-- Search Demo -->

- <logos-svelte-icon /> **搜索知識庫** - 搜索 Svelte 官方文檔  <!-- **Search Knowledge Base** - Search Svelte Official Documentation -->
- 🤖 **AI** - 使用 OpenAI ChatGPT-3.5 <!-- **AI** - Use OpenAI ChatGPT-3.5  -->
- 🤹 **任何語言** - 它將使用相同的語言回复 <!-- **Any Language** - It will reply using the same language of the question -->
<!-- - 📖 -->

95+ 語言: <span class="text-blue">**繁體中文**</span>、簡體中文、英語、日語、韓語、印度尼西亞語、西班牙語、法語、德語、阿拉伯語、荷蘭語、越南語、泰語、阿爾巴尼亞語、亞美尼亞語、阿瓦迪語、阿塞拜疆語、巴什基爾語、巴斯克語、白俄羅斯語、孟加拉語、博傑普爾語、波斯尼亞語、巴西語 葡萄牙語、保加利亞語、加泰羅尼亞語、恰蒂斯加爾希語、克羅地亞語、捷克語、丹麥語、多格里語、愛沙尼亞語、法羅語、芬蘭語、加利西亞語、格魯吉亞語、希臘語、古吉拉特語、哈里亞納語、印地語、匈牙利語、愛爾蘭語、意大利語、爪哇語、卡納達語、克什米爾語、邁蒂利語、馬來語、廣東話、吳語、等

<!-- Source: https://seo.ai/blog/how-many-languages-does-chatgpt-support -->

---

# 用例 <!-- Use Cases -->

創建一個非常厲害的聊天助手： <!-- Create a highly proficient chat assistant for your: -->

- 公司內部知識庫 <!-- company internal knowledge base -->
- 產品文檔 <!-- product documentation -->
- 個人知識庫 <!-- personal knowledge base -->

---

# 怎麼構建知識庫搜索

- <clarity-process-on-vm-line inline /> 將知識庫預處理成小部分 <!-- Preprocess knowledge base into small sections -->
- <carbon-3d-print-mesh /> 使用 OpenAI 創建 Embeddings <!-- Create embeddings using OpenAI -->
- <carbon-search /> 前端：添加搜索輸入 <!-- Add a search box to the front end -->
- <carbon-bare-metal-server /> 創建 SvelteKit 後端 API 端點 <!-- Create SvelteKit back-end API endpoint  -->
  - <carbon-3d-print-mesh /> 創建用戶問題的 Embedding <!-- Create embedding of user's query -->
  - <fluent-mdl2-documentation /> 使用 Embeddings 來查找最相關的文檔部分 <!-- Use embeddings to find most relevant documentation sections -->
  - <carbon-chat /> 將相關文檔和用戶問題發送給 OpenAI <!-- Send relevant documentation and user's question to OpenAI -->
  - <fluent-stream-output-20-regular /> 將答案流式傳輸給用戶 <!-- Stream answer back to user -->
- <mdi-message-text-fast-outline /> 顯示答案 <!--Show answer-->
- <logos-firebase/> 使用 Firebase Auth 保護您的 API <!-- Secure your API with Firebase Auth -->

---
src: ./pages/preprocess.md
---

---
src: ./pages/create-embeddings.md
---

---

# What is an embedding?

Explain if time

---
src: ./pages/search-box.md
---

---
src: ./pages/api-endpoint.md
---

---
src: ./pages/secure-with-firebase.md
---

---

# 未來的任務

- <logos-firebase/> 將問題和答案保存到 Firestore
- 計數使用
- 如果您的知識庫很大並且您想快速檢索最相關的文檔部分，您可以使用 [Google Cloud Matching Engine Vertex AI](https://cloud.google.com/blog/topics/developers-practitioners/find-anything-blazingly-fast-googles-vector-search-technology) 或任何 [OpenAI 建議的矢量數據庫](https://platform.openai.com/docs/guides/embeddings/how-can-i-retrieve-k-nearest-embedding-vectors-quickly)

<!-- This will allow users to view their history and you can provide cached answers to nearly identical questions. -->

---

# 了解更多

- <carbon-logo-github /> 查看源代碼: [**polylingual.dev**](https://github.com/jacob-8/polylingual.dev)
<br />

- [OpenAI Introducing text and code embeddings](https://openai.com/blog/introducing-text-and-code-embeddings/), [OpenAI Emebeddings Documentation](https://platform.openai.com/docs/guides/embeddings/what-are-embeddings)
- [Storing OpenAI embeddings in Postgres with pgvector](https://supabase.com/blog/openai-embeddings-postgres-vector), [Supabase Clippy: ChatGPT for Supabase Docs](https://supabase.com/blog/chatgpt-supabase-docs)
- [Nearest Neighbor Search](https://towardsdatascience.com/using-approximate-nearest-neighbor-search-in-real-world-applications-a75c351445d)
- [Vertex AI Matching Engine overview](https://cloud.google.com/vertex-ai/docs/matching-engine/overview)
- [Visualize Embeddings](https://nnext.ai/wiki/Visualizing-ChatGPT-embeddings-2ecbf1423280479fa6f303c3343a49a1)
- [sqlite-vss: A SQLite Extension for Vector Search](https://observablehq.com/@asg017/introducing-sqlite-vss)

---

# 有問題嗎?

---
layout: iframe-right
url: https://polylingual.dev
---

# 有問題嗎?

- Want to try it out?

<div mb-10 />

# 聯繫我

- <logos-twitter/> [@jacobbowdoin](https://twitter.com/jacobbowdoin)
- <logos-facebook/> [了解 JavaScript 高雄社團](https://www.facebook.com/groups/liaojiejavascript): 每週一晚上七點到九點我們開會。

<img ml-6 mt-4 w-180px border="rounded" src="/facebook-qr.png">


---
