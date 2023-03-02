## <clarity-process-on-vm-line inline /> 將知識庫預處理成小部分 <!-- Preprocess knowledge base into small sections -->

<div flex flex-wrap space-x-4>
<div>
```ts {all|2|3|4|5|6}
async function process_doc(filepath: string): Promise<ProcessedDoc> {
  const markdown = readFileSync(filepath, 'utf8');
  const sections = parse_markdown_sections(markdown);
  const sections_with_hashes = add_hashes(sections);
  const sections_with_tokens = add_tokens(sections_with_hashes);
  const sections_with_embeddings = await get_embeddings(sections_with_tokens);
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

<!-- In my project, I take these processed docs and save them to a CSV. In a production application with many documents, you would want store them in vector database and I will share links to suggestions later. -->