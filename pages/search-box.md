## <carbon-search /> 前端：添加搜索輸入 <!-- Add a search box to the front end -->

```svelte {all|4,8,10,19|5,14,23|8}
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
This will send a query to our backend and listen for answers
Last line: Now, let's create the backend API for this route. Since we're using SvelteKit, that's REALLY easy. -->