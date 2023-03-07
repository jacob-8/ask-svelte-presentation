## <clarity-process-on-vm-line inline /> 把文檔預先處理成小部分 <!-- Preprocess documentation into small sections -->

<div flex flex-wrap space-x-4>
<div>
```ts {all|2|3|4|5|6}
async function process_doc(filepath: string): Promise<ProcessedDoc> {
  const markdown = readFileSync(filepath, 'utf8');
  const sections = parse_markdown_sections(markdown);
  const sections_with_hashes = add_hashes(sections);
  const sections_with_tokens = add_tokens(sections_with_hashes);
  const sections_with_embeddings = await add_embeddings(sections_with_tokens);
  return {
    filepath,
    sections: sections_with_embeddings,
  };
}
```
</div>

<div>
```ts
interface ProcessedDoc {
  filename: string;
  sections: Section[];
}

interface Section {
  title: string;
  content: string;
  hash?: string; // "ajd82j4k"
  token_count?: number; // 241
  embedding?: number[];
}
```
</div>
<div v-if="$slidev.nav.clicks === 2">
```ts
function parse_markdown_sections(markdown: string): Section[] {
  const sections = [];
  const ast = unified().use(remarkParse).parse(markdown);
  ast.children.forEach((node) => {
    if (node.type === 'heading') {
      // handle section title
    } else {
      // handle section content
    }
  });
  return sections;
}
```
</div>

<div v-if="$slidev.nav.clicks === 3">
```ts
function add_hashes(sections: Section[]): Section[] {
  return sections.map(section => {
    ...section,
    hash: murmurHash3(section.content),
  })
}
```
</div>

<div v-if="$slidev.nav.clicks === 4">
```ts
const tokenizer = new GPT3Tokenizer({ type: 'gpt3' });

function add_tokens(sections: Section[]): Section[] {
  return sections.map(section => {
    ...section,
    token_count: tokenizer.encode(section.title + ' ' + section.content).text.length,
  })
}
```
</div>

<div v-click=5>
```ts {all|4|9|4}
async function add_embeddings(sections: Section[]): Promise<Section[]> {
  const sections_with_embeddings: Section[] = [];
  for (const section of sections) {
    const embedding = await generate_embedding(section.title + ' ' + section.content);
    sections_with_embeddings.push({
      ...section,
      embedding,
    });
    await sleep(2000); // to avoid OpenAI too-fast errors
  }
}
```
</div>
</div>

<!-- 怎麼把文檔預先處理成小部分呢?

這是預先處理過程的簡化版本. 我把每個 SvelteKit 文檔的 markdown 文件上執行(zhíxíng)這個函數:

(click each bullet)
- 讀文件內容
- 使用 Remark 和 Unified 處理路徑按 markdown 標題解析為部分
- 根據內容為每個部分創建一個 hash, 這個 hash 是部分的 ID. 我可以在將來使用它來知道部分內容有沒有變化。
- 然後算數一個部分有多少個 tokens。這是 OpenAI 用於計費使用的文本長度度量。
- 然後把每個部分生成一個 embedding...
  - 使用 OpenAi 的 embedding API
  - 請注意，我們在請求之間等待 2 秒(3rd)，免得請求速度過太快而發生錯誤。
- 在我的項目中，我把這些處理過的部分儲存到 CSV 文件中。 

Here is a simplified version of the process I run for every markdown file in the SvelteKit documentation.
- Read the file contents
- Parse into section by markdown heading using Remark and the Unified processing pipeline
- Create a hash for each section based on it's content - this will serve as the section ID and I can use it in the future to know if a section's content have changed.
- Then I count how many tokens a section is. This is a measure of text length that OpenAI uses for billing usage.
- Then I generate an embedding for each section...
  - using OpenAI's Embeddings endpoint
  - with an important pause after each request to avoid errors for making requests too many requests at once
- In my project, I take these processed sections and save them to a CSV. In a production application with many documents, you would want store them in vector database and I will share links to suggestions later.
-->

---

## CSV 結果

<img m="y-5" border="rounded" src="/csv-data.png">

<!-- 
在包含許多文檔的生產應用程式中，你可能希望把它們儲存在矢量數據庫中，然後我會分享一些建議的矢量數據庫。

那麼 embeddings 什麼是 ? 

So what is an embedding? -->