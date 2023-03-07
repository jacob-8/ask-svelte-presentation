## <carbon-bare-metal-server /> 後端：創建 SvelteKit API 端點 <!-- Create SvelteKit back-end API endpoint  -->

<ul>
<li>
<span v-if="$slidev.nav.clicks > 0">
<carbon-3d-print-mesh /> 把使用者的問題創建 embedding <!-- Create embedding of user's query -->
</span>
</li>
<li>
<span v-if="$slidev.nav.clicks > 1">
<fluent-mdl2-documentation /> 使用 embedding 來查找最相關的文檔部分 <!-- Use embeddings to find most relevant documentation sections -->
</span>
</li>
<li>
<span v-if="$slidev.nav.clicks > 2">
<carbon-chat /> 把相關文檔部分和使用者的問題發送給 OpenAI <!-- Send relevant documentation and user's question to OpenAI -->
</span>
</li>
</ul>

```ts {1,3,18|4-5|2,6-7|8,11-14|9-10|15|19}
// src/routes/api/query/+server.ts
const MAX_DOCS_TO_RETURN = 10;
export const POST: RequestHandler = async ({ request, fetch }) => {
  const { query } = await request.json()
  const query_embedding = await generate_embedding(query)
  const doc_sections = await load_doc_sections('sveltejs.kit/docs.csv', fetch)
  const nearest_matches = find_closest_embeddings_cosine(query_embedding, doc_sections, MAX_DOCS_TO_RETURN)
  const document_context = concat_matched_documents(nearest_documents)
  const completionResponse = await openai.createCompletion({
    model: 'gpt-3.5-turbo',
    messages: [
        { role: 'system', content: system_message },
        { role: 'user', content: generate_user_prompt(document_context, query) },
      ],
    max_tokens: 1000,
    temperature: 0,
  })
  const { choices: [{ text }] } = completionResponse.data
  return json(text)
}
```

<div fixed bottom-10 left-10 right-10 border="~ green" shadow v-if="$slidev.nav.clicks === 2">
```ts
function cosine_similarity(a: number[], b: number[]): number {
  const dotProduct = a.reduce((acc, ai, i) => acc + ai * b[i], 0);
  const magnitudeA = Math.sqrt(a.reduce((acc, ai) => acc + ai ** 2, 0));
  const magnitudeB = Math.sqrt(b.reduce((acc, bi) => acc + bi ** 2, 0));
  const similarity = dotProduct / (magnitudeA * magnitudeB);
  return similarity;
}
```
</div>

<div fixed top-2 left-10 right-10 border="~ green" shadow v-if="$slidev.nav.clicks === 3">
```ts
const system_message = "You love to help people understand how to use Svelte and SvelteKit to build full-stack websites.";

function generate_user_prompt(document_context: string, query: string)
  return `Given the following context sections, answer the question using only that information, written in markdown format. If you are unsure and the answer is not explicitly written in the context sections, say "Sorry, I don't know how to help with that."

Context sections:
${document_context}

Question: """
${query}
"""

Answer as markdown in the same language as the question (including related code snippets if available):
`
```
</div>

<style>
  code {
    white-space : pre-wrap !important;
  }
</style>

<!-- Helpful resource [prmpts.ai](https://prmpts.ai/) -->

<!-- 
- 我要加新的 SvelteKit 的服務器路由: `/api/query` 所以我創建這個文件 `routes/api/query/+server.ts` 
- 我要把使用者的問題創建 embedding 所以我利用一樣的創建 embeddings 的方法.
- 加載有 embeddings 的文檔部分，和找到與問題的意思最近的那些部分。
  - 通過計算使用者的問題與文檔的每個部分之間的 cosine 相似度來執行(zhíxíng)此操作(cāozuò)，並留 10 個左右最相似的部分。 現在我的 SvelteKit 文檔部分 embeddings 的 CSV 文件是 5MB。 目前使用這種大小的 CSV 文件是可以的，但在某些時候我需要使用支持(zhīchí)矢量(shǐliàng)的數據庫(shùjùkù)來儲存和找最接近的 embeddings。 使用 CSV 是一種很好的開始方法，不需要用矢量數據庫。
- 然後把相關文檔部分和使用者的問題生成提示, prompt。
  - 首先給 ChatGPT 一個角色，然後給出指令(zhǐlìng)，相關文檔部分，使用者的問題，還有更多指令。最後的指令非常重要: 用問題的語言生成答案和包括相關的代碼.
- 創建 OpenAI 聊天請求
  - 根據你的條件使用 `max_tokens` 來限制。
- 把 OpenAI 的答案發送到前端。如果你查看我的原代碼，你會發現我是在流式傳輸(chuánshū)答案，這不是需要的，但可以提供更好的經驗。
- 那,怎麼保護這個 API？


- Create a server route that will be available at `/api/query` by creating a file in `src/routes/api/query/+server.ts`
- Generate an embedding for the user's query using the same method you did to create embeddings for your documentation sections
- Load in your documentation sections which have embeddings and find those sections which are closest in meaning to the user's query. 
  - Do this by calculating the cosine similarity between your user's query and each section of your documentation and keep the 10 or so most similar. My CSV file of SvelteKit documentation section embeddings is 5MB. Using a CSV file of this size is OK for now, but at some point I'll need to use a vector capable database to store and fetch the closest embeddings. Using a CSV is a good way to get started without having to connect to a vector searching database.
- Now we take those sections and the user's question to generate a prompt.
  - First we give the chatbot a role, then we give instruction, our document_content that we concatenated, the user's question, and then a bit more instruction following by a colon.
- Create an OpenAI chat request
  - Use a `max_tokens` limit based on your needs. I recommend giving the chatbot a limit in your instructions rather than here as it's very disorienting for user's to have an answer cut short if it is a long one.
- Take OpenAI's response and send it back to the frontend. If you view my code repo, you'll see that I'm actually streaming the answer back, which is not necessary but makes for a nicer user experience.
- How do we secure this API?
-->


