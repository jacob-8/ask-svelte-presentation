## <carbon-3d-print-mesh /> 使用 OpenAI 創建 Embeddings <!-- Create embeddings using OpenAI -->

```ts {all|13}
import { Configuration, OpenAIApi } from "openai";

async function generate_embedding(text: string): Promise<number[]> {
  const configuration = new Configuration({ apiKey: process.env.OPENAI_API_KEY });
  const openai = new OpenAIApi(configuration);

  const embedding_response = await openai.createEmbedding({
    model: 'text-embedding-ada-002',
    input: text,
  })

  const [{ embedding }] = embedding_response.data.data;
  return embedding; // [-0.02791161,0.018015675,-0.015331353,0.0069779027,0.00460408,0.018162578,-0.013808901, ...]
}
```
1,000 tokens is about 750 English words or 450 Chinese characters.
1,000 tokens = $0.0004 USD
1元 = 82500 tokens

The following paragraph has 137 characters and 178 tokens:

了解我如何使用 OpenAI 和 SvelteKit 為 Svelte 文檔站點構建類似 ChatGPT 的多語言對話搜索。
了解我如何使用 Firebase Auth 保護我的 API，並獲得一些關於如何使用 Firebase 為您自己或您的公司創建多語言搜索工具的想法。

<!-- Could just pull this link into an iframe https://platform.openai.com/tokenizer -->