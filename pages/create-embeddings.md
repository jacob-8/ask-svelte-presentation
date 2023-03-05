## <carbon-3d-print-mesh /> 使用 OpenAI 創建 Embeddings <!-- Create embeddings using OpenAI -->

```ts {all|1,4-5|7-10|13|all}
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

<!--
1. 使用你的 API 密鑰初始化 OpenAIApi
2. 把字符串 (zìfú chuàn) 發送到 OpenAI
3. 接收 embedding, OpenAI 的 embeddings 有 1536 個維度/方面

- 請注意，這些代碼示例沒有導入(dǎorù)和錯誤捕獲(bǔhuò)，讓我比較容易給大家解釋，但然後請閱讀源代碼, 它有導入和錯誤捕獲。

1. Init OpenAIApi using your API key
2. Send the string to OpenAI
3. Receive back the embedding, an array with values between -1 and 1 for each of the 1536 dimensions that OpenAI uses in their embeddings.
- Note that I've left out all imports and error catching in my code samples to make it easier to display on screen and explain, but please read the source code to find the imports and make sure to catch errors.  
-->