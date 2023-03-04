## <carbon-bare-metal-server /> 後端：創建 SvelteKit API 端點 <!-- Create SvelteKit back-end API endpoint  -->

- <carbon-3d-print-mesh /> 把用戶的問題創建矢量數據 <!-- Create embedding of user's query -->
- <fluent-mdl2-documentation /> 使用矢量數據來查找最相關的文檔部分 <!-- Use embeddings to find most relevant documentation sections -->
- <carbon-chat /> 把相關文檔部分和用戶的問題發送給 OpenAI <!-- Send relevant documentation and user's question to OpenAI -->

```ts {all|4-5|6-7|8-9|8-9|10-17}
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

<!-- Helpful resource [prmpts.ai](https://prmpts.ai/) -->

<!-- 
1. Create a server route that will be available at /api/query by creating a file in src/routes/api/query/+server.ts 
2. Generate an embedding for the user's query
3. Load in your documentation sections which have embeddings and find those sections which are closest in meaning to the user's query. This is really quite simple to do by calculating the cosine similarity between your user's query and each section of your documentation. Simply keep those that are most similar. My CSV file of embeddings is currently 5MB for the SvelteKit documentation website. It works, but pretty soon I'll need to use a vector capable database to store and fetch the nearest embeddings. But this is a good way to get started without having to choose a vector searching service.
4. Now we generate a prompt and this is very important. First we give the chatbot a role, then we give instruction, our document_content that we concatenated, the user's question, and then a bit more instruction following by a colon.
5. Send that request off, with a maximum limit that you determine best based on your needs. I recommend giving the chatbot a limit in your instructions rather than here as it's very disorienting for user's to have an answer cut short if it is a long one.
6. That response is the answer you remember we awaited and displayed to the user, expecting to receive markdown. If you view my code repo, you'll see that I'm actually streaming the answer back, which is not necessary but makes for a nicer user experience.
-->


