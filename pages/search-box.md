## <carbon-search /> 前端：添加搜索輸入 <!-- Add a search box to the front end -->

```svelte {all|5,9,11,19|6,14,23|9}
<!-- src/routes/query/+page.svelte -->
<script lang="ts">
  import SvelteMarkdown from 'svelte-markdown';
  
  let query = '';
  let answer: string;
  
  async function query_documentation() {
    const response = await fetch('/api/query', {
      method: 'POST',
      body: JSON.stringify({ query }),
      headers: { 'content-type': 'application/json' }
    });
    answer = await response.json();
  }
</script>

<form on:submit={query_documentation}>
  <input type="search" bind:value={query} placeholder="Ask me anything about Svelte..." />
</form>

{#if answer}
  <SvelteMarkdown source={answer} />
{/if}
```

<!-- 
然後在前端：添加搜索輸入
1. Create a new SvelteKit route that will display at `/query` by adding `routes/query/+page.svelte`
2. Add an input, bind the value to the query variable and send that to our backend API using a fetch request
3. When we receive the answer we'll display the markdown to the user.

Now, let's create the backend API for `/api/query`. Since we're using SvelteKit, it's REALLY easy. -->