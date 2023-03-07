---
theme: seriph
background: https://source.unsplash.com/fVBWN3_ST0E/1920x1080
class: 'text-center'
highlighter: shiki
lineNumbers: false
info: |
  ## Ask SvelteKit Presentation for March 2023 TOOCON in Kaohsiung, Taiwan
drawings:
  persist: false
css: unocss
---

# 問 SvelteKit <!-- Ask SvelteKit -->

以任何語言來使用 AI 搜索知識庫 <!-- Use AI to search a knowledge knowledge using any language -->

<div class="pt-12">
  <span @click="$slidev.nav.next" class="px-2 py-1 rounded cursor-pointer text-gray-400" hover="bg-white/10">
    按空格鍵 <!-- Press Space for next page  -->
    <carbon:arrow-right class="inline"/>
  </span>
</div>

<div class="abs-br m-6 flex gap-2">
  <a href="https://github.com/jacob-8/ask-svelte-presentation" target="_blank" title="GitHub"
    class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-logo-github />
  </a>
</div>

<style>
p {
  font-size: 210%;
}
</style>

<!--
大家好! 我叫亞瀾. 今天我很高興給大家一個 AI 的介紹。 我很感謝 戴維廷(Dài Wéitíng) 給我這個機會。 大概 2004，我開始做網站。 到目前我還沒有關於這方面技術(jìshù)的演講經驗。 有一次我給一個演講關於技術，但我的觀眾不是程式人。 所以這是我第一次真的關於技術演講。我正在學習中文，所以我想，為什麼不用中文進行我第一次技術的演講呢？ 那以後如果我用英文演講的時候，我會覺得很容易。

在我開始之前，我需要感謝我太太麗婷(Lìtíng)，感謝她的支持還有興趣。

那我們開始把。 我覺得你們已經知道怎麼用 ChatGPT，但你知道怎麼把 ChatGPT 放在自己的工具?

今天我會幫助你了解我如何使用 OpenAI 和 SvelteKit 為 Svelte 文檔網站構建類似 ChatGPT 的多語言對話搜索。還有了解我如何使用 Firebase Auth 保護我的 API。最後我希望你獲得一些關於如何使用 Firebase 為你自己或你的公司創建多語言搜索工具的想法。
-->

---
layout: iframe-right
url: https://kit.svelte.dev
---

# 搜索示範

<div overflow-x-scroll h-450px v-click>
<img m="y-10" border="rounded" src="/samples/how-do-i-create-an-api_zh.png">

[polylingual.dev/zh-TW/search](https://polylingual.dev/zh-TW/search)
</div>

<!-- 首先，我會示範我構建的聰明的搜索引擎，然後怎麼構建它。Svelte 是一個 JavaScript 框架，還有你可以利用 SvelteKit 做厲害的 web 應用程式 (好像 NextJS)。 那 SvelteKit 沒有中文的文檔。 因為內容常常變化, 翻譯文檔很難。如果我的英文不太好怎麼辦學怎麼用 SvelteKit. 比如怎創建一個 API 路由.有一點難. demonstrate how to find*

這將不會再是問題。 使用我的工具，我們可以獲取英文文檔，用中文提出問題，然後得到有用的中文答案。 -->

---
layout: iframe-right
url: https://polylingual.dev/zh-TW/search
---

# 搜索示範 <!-- Search Demo -->

- <logos-svelte-icon /> **搜索知識庫** - 搜索 SvelteKit 官方文檔  <!-- **Search Knowledge Base** - Search SvelteKit Official Documentation -->
- 🤖 **AI** - 使用 OpenAI ChatGPT-3.5 <!-- **AI** - Use OpenAI ChatGPT-3.5  -->
- 🤹 **任何語言** - 它將使用相同的語言回复 <!-- **Any Language** - It will reply using the same language of the question -->

95+ 語言: <span class="text-blue">**繁體中文**</span>、簡體中文、英語、日語、韓語、印度尼西亞語、西班牙語、法語、德語、阿拉伯語、荷蘭語、越南語、泰語、阿爾巴尼亞語、亞美尼亞語、阿瓦迪語、阿塞拜疆語、巴什基爾語、巴斯克語、白俄羅斯語、孟加拉語、博傑普爾語、波斯尼亞語、巴西語 葡萄牙語、保加利亞語、加泰羅尼亞語、恰蒂斯加爾希語、克羅地亞語、捷克語、丹麥語、多格里語、愛沙尼亞語、法羅語、芬蘭語、加利西亞語、格魯吉亞語、希臘語、古吉拉特語、哈里亞納語、印地語、匈牙利語、愛爾蘭語、意大利語、爪哇語、卡納達語、克什米爾語、邁蒂利語、馬來語、廣東話、吳語、等

<!-- 讓我問幾個問題。 

1. How do I create a page route? 如何創建頁面路由？
2. What is the difference between Svelte and SvelteKit? Svelte 和 SvelteKit 有什麼不同？
3. What are snapshots?, 什麼是快照？, 什麼是 snapshots？, 什麼是快照？ 請舉個例子

最後我會讓你們試試看，所以如果你有使用 SvelteKit、NextJS、Nuxt、Astro 或任何 JavaScript 框架的經驗，請開始考慮你想問什麼問題。

Demo questions:
1. How do I create an api? 如何創建 api？, 如何創建 api？ 告訴我如何在我的應用程式中使用它
2. How do I create a layout reset?, 如何創建佈局重置？
3. tell me about layout resets, 告訴我關於 layout resets, 告訴我關於 layout resets 並給我一個例子
4. What is the +error file?, +error 文件是什麼？
5. What is SvelteKit useful for? SvelteKit 有什麼用？
6. How do you write links? 如何寫鏈接？
7.  What is a route matcher? How do I create a route matcher? // no longer works: How do I create a route matcher for the values "es" and "de"?
8.  How do I restrict a route to certain parameters? 如何限制路由的某些參數？
9.  Funny: Who created SvelteKit? 誰創造了 SvelteKit？
10. Can't help: How do you spin routes backwards? 如何反向旋轉路由？

[95 languages source](https://seo.ai/blog/how-many-languages-does-chatgpt-support) -->

---

# 用例 <!-- Use Cases -->

創建一個非常厲害的聊天助手： <!-- Create a highly proficient chat assistant for your: -->

- 公司內部知識庫 <!-- company internal knowledge base -->
- 產品文檔 <!-- product documentation -->
- 個人知識庫 <!-- personal knowledge base -->

<style>
h1 {
  font-size: 220%;
  margin-bottom: 40px;
}
* {
  font-size: 140%;
}
</style>

<!-- 這個科技有很多領用. 

READ 3 bullets. 

你只有一個限制:你的想像力。-->

---

# 怎麼構建知識庫搜索

<v-clicks>

- <clarity-process-on-vm-line inline /> 把文檔預先處理成小部分 <!-- Preprocess documentation into small sections -->
- <carbon-3d-print-mesh /> 使用 OpenAI 創建 embeddings <!-- Create embeddings using OpenAI -->
- <carbon-search /> 前端：添加搜索輸入 <!-- Add a search box to the front end -->
- <carbon-bare-metal-server /> 後端：創建 SvelteKit API 端點 <!-- Create SvelteKit back-end API endpoint  -->
  - <carbon-3d-print-mesh /> 把使用者的問題創建 embedding <!-- Create embedding of user's query -->
  - <fluent-mdl2-documentation /> 使用 embedding 來查找最相關的文檔部分 <!-- Use embeddings to find most relevant documentation sections -->
  - <carbon-chat /> 把相關文檔部分和使用者的問題發送給 OpenAI <!-- Send relevant documentation and user's question to OpenAI -->
  - <fluent-stream-output-20-regular /> 把答案流式傳輸給使用者 <!-- Stream answer back to user -->
- <mdi-message-text-fast-outline /> 前端：示範答案 <!--Show answer-->
- <logos-firebase/> 使用 Firebase Auth 保護 API <!-- Secure your API with Firebase Auth -->

</v-clicks>

<style>
li {
  font-size: 1.5rem;
}
</style>

<!-- 首先...

chuánshū -->

---
src: ./pages/preprocess.md
---

---
src: ./pages/what-are-embeddings.md
---

---
src: ./pages/create-embeddings.md
---

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

<v-clicks>

- <logos-firebase/> 把問題和答案保儲存到 Firestore
- <uil-tachometer-fast/> 計數使用
- <carbon-3d-print-mesh /> 使用 embeddings 搜索引擎 
  - [Google Cloud Matching Engine Vertex AI](https://cloud.google.com/blog/topics/developers-practitioners/find-anything-blazingly-fast-googles-vector-search-technology) 
  - 任何 [OpenAI 建議的矢量數據庫](https://platform.openai.com/docs/guides/embeddings/how-can-i-retrieve-k-nearest-embedding-vectors-quickly)

</v-clicks>

<style>
li {
  font-size: 1.5rem;
}
</style>

<!-- 
現在我們的搜索成功了，但對於像這樣的項目，我們可以採取(cǎiqǔ)一些合乎(héhū)邏輯(luójí)的後續步驟(bùzhòu)。

(click and read bullets)

- ...所以使用者可以查看他們的歷史
- ...了解使用者或收取使用費
- 如果你的知識庫很大並且你想快速檢索(jiǎnsuǒ)最相關的文檔部分，你可以... 

That will create a working tool, but there are some logical next steps we could take with a project like this...
-->

---

# 了解更多

- <carbon-logo-github /> 查看源代碼: [**jacob-8/polylingual.dev**](https://github.com/jacob-8/polylingual.dev)
<br />

- [OpenAI Introducing text and code embeddings](https://openai.com/blog/introducing-text-and-code-embeddings/), [OpenAI Emebeddings Documentation](https://platform.openai.com/docs/guides/embeddings/what-are-embeddings)
- [Storing OpenAI embeddings in Postgres with pgvector](https://supabase.com/blog/openai-embeddings-postgres-vector), [Supabase Clippy: ChatGPT for Supabase Docs](https://supabase.com/blog/chatgpt-supabase-docs)
- [Nearest Neighbor Search](https://towardsdatascience.com/using-approximate-nearest-neighbor-search-in-real-world-applications-a75c351445d)
- [Vertex AI Matching Engine overview](https://cloud.google.com/vertex-ai/docs/matching-engine/overview)
- [Perform Semantic Search on a Postgres database](https://nnext.ai/wiki/Introducing-pgvector---Perform-Semantic-Search-on-a-Postgres-database-e114fca6811c4583a6d516eca80ff42b)
- [sqlite-vss: A SQLite Extension for Vector Search](https://observablehq.com/@asg017/introducing-sqlite-vss)

<!-- 那, 因為從這個工具你剛剛獲得一些知識，當你有空，我希望你能花一些時間來探索(tànsuǒ)源代碼和親自試試一下。 明天我會在 TOOCON Facebook 頁面上發布這個 Slidev PPT，你可以用這些鏈接(liànjiē)查看我的項目的源代碼或閱讀這些有用的文章。 -->

---

# 有問題嗎?

---
layout: iframe-right
url: https://polylingual.dev/zh-TW/search
---

# 有問題嗎?

- 要不要試試看? <!-- Want to try it out? -->

<div mb-10 />

# 聯繫我

- <logos-twitter/> [@jacobbowdoin](https://twitter.com/jacobbowdoin)
- <logos-facebook/> [了解 JavaScript 高雄社團](https://www.facebook.com/groups/liaojiejavascript): 每週一晚上七點到九點我們開會。

<img ml-12 mt-4 w-180px border="rounded" src="/facebook-qr.png">

<!-- 掃描(sǎomiáo) QR code 進入我的 Facebook 社團。你會看到一個討論區，請把你的問題放在那邊(用中文)。然後我會把你的問題放在這個輸入所以我們可以一起問 SvelteKit.

liánxì 

Demo questions:
1. How do I create a page route? 如何創建頁面路由？
2. How do I create an api? 如何創建 api？, 如何創建 api？ 告訴我如何在我的應用程式中使用它
3. What are snapshots?, 什麼是快照？, 什麼是 snapshots？, 什麼是快照？ 請舉個例子
4. What is the difference between Svelte and SvelteKit? Svelte 和 SvelteKit 有什麼不同？
5. How do I create a layout reset?, 如何創建佈局重置？
6. tell me about layout resets, 告訴我關於 layout resets, 告訴我關於 layout resets 並給我一個例子
7. What is the +error file?, +error 文件是什麼？
8. What is SvelteKit useful for? SvelteKit 有什麼用？
9. How do you write links? 如何寫鏈接？
10. What is a route matcher? How do I create a route matcher? // no longer works: How do I create a route matcher for the values "es" and "de"?
11. How do I restrict a route to certain parameters? 如何限制路由的某些參數？
12. Funny: Who created SvelteKit? 誰創造了 SvelteKit？
13. Can't help: How do you spin routes backwards? 如何反向旋轉路由？
14. Half right, half wrong: Tell me about .env variable files >> 跟 ChatGPT 這個工具有一樣的弱點, 缺點。有時候寫錯,所以我會加文檔源鏈接。

-->

---
