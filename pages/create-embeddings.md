## <carbon-3d-print-mesh /> 使用 OpenAI 創建矢量數據 (Embeddings) <!-- Create embeddings using OpenAI -->

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
1. Init OpenAIApi using your API key
2. Send the text to OpenAI
3. Receive back the embedding, an array with values between -1 and 1 for each of the 1536 dimensions that OpenAI uses in their embeddings.
- Note that I've left out all imports and error catching in my code samples to make it easier to display on screen and explain, but please read the source code to find the imports and make sure to catch errors.  -->