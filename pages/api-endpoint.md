## <carbon-bare-metal-server /> 後端：創建 SvelteKit API 端點 <!-- Create SvelteKit back-end API endpoint  -->

<ul>
<li v-if="$slidev.nav.clicks > 0">
<carbon-3d-print-mesh /> 把用戶的問題創建矢量數據 <!-- Create embedding of user's query -->
</li>
<li v-if="$slidev.nav.clicks > 1">
<fluent-mdl2-documentation /> 使用矢量數據來查找最相關的文檔部分 <!-- Use embeddings to find most relevant documentation sections -->
</li>
<li v-if="$slidev.nav.clicks > 2">
<carbon-chat /> 把相關文檔部分和用戶的問題發送給 OpenAI <!-- Send relevant documentation and user's question to OpenAI -->
</li>
</ul>

```ts {1,3,18|4-5|2,6-7|8-9|8-9|10-17}
// src/routes/api/query/+server.ts
const MAX_DOCS_TO_RETURN = 15;
export const POST: RequestHandler = async ({ request, fetch }) => {
  const { query } = await request.json()
  const query_embedding = await generate_embedding(query)
  const doc_sections = await load_doc_sections('sveltejs.kit/docs.csv', fetch)
  const nearest_matches = find_closest_embeddings_cosine(query_embedding, doc_sections, MAX_DOCS_TO_RETURN)
  const document_context = concat_matched_documents(nearest_documents)
  const prompt = generate_prompt(document_context, query)
  const completionResponse = await openai.createCompletion({
    model: 'gpt-3.5-turbo',
    prompt,
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

<div fixed top-2 left-10 right-10 border="~ green" shadow v-if="$slidev.nav.clicks === 4">
```ts
function generate_prompt(document_context: string, query: string)
  return `You love to help people understand how to use Svelte and SvelteKit to build full-stack websites. Given the following context sections from various documentation sites, answer the question using only that information, outputted in markdown format. If you are unsure and the answer is not explicitly written in the context sections, say "Sorry, I don't know how to help with that."

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
- Create a server route that will be available at `/api/query` by creating a file in `src/routes/api/query/+server.ts`
- Generate an embedding for the user's query using the same method you did to create embeddings for your documentation sections
- Load in your documentation sections which have embeddings and find those sections which are closest in meaning to the user's query. This is really quite simple to do by calculating the cosine similarity between your user's query and each section of your documentation. Simply keep those that are most similar. My CSV file of embeddings is currently 5MB for the SvelteKit documentation website. It works well at this size, but at some point I'll need to use a vector capable database to store and fetch the nearest embeddings. But using a CSV is a good way to get started without having to connect to a vector searching database.
- Now we take those sections and the user's question to generate a prompt.
- First we give the chatbot a role, then we give instruction, our document_content that we concatenated, the user's question, and then a bit more instruction following by a colon.
- Send that request to OpenAI, with a `max_tokens` limit that you determine based on your needs. I recommend giving the chatbot a limit in your instructions rather than here as it's very disorienting for user's to have an answer cut short if it is a long one.
- We then take OpenAI's response and send it back to the frontend. If you view my code repo, you'll see that I'm actually streaming the answer back, which is not necessary but makes for a nicer user experience.
- How do we secure this API?
-->


