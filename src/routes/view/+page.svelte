<script lang="ts">
  import View from '$lib/components/View.svelte';
  import { initHandlerV2, fetchJSON } from '$lib/util/util';
  import { onMount } from 'svelte';
  import { page } from '$app/stores';
  import type { APIResponse } from '$lib/types';
  import { derived } from 'svelte/store';
  // onMount(initHandler);
  let apiData: APIResponse = {
    code: ''
  }; // Variable to store API response

  const urlParameters = derived(page, ($page) => $page.url.searchParams);
  const pageId = derived(urlParameters, (parameters) => parameters.get('pageId'));
  const contentId = derived(urlParameters, (parameters) => parameters.get('contentId'));

  onMount(async () => {
    try {
      apiData = await fetchJSON(`/api/mermaid/content?pageId=${$pageId}&contentId=${$contentId}`); // Fetch data from the API
      initHandlerV2(apiData); // Run initHandler after the data is fetched
    } catch (error) {
      console.error('Failed to initialize:', error);
    }
  });
</script>

<View />
