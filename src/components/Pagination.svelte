<script>
  import { createEventDispatcher } from 'svelte'

  const dispatch = createEventDispatcher()

  export let totalItems = 0
  export let pageSize = 1
  export let currentPage = 1

  function range(size, startAt = 0) {
    return [...Array(size).keys()].map(i => i + startAt);
  }

  $: lastPage = Math.ceil(totalItems / pageSize)

  // handle next/prev clicks
  function handleOptionClick (option) {
    if(option !== currentPage){
    dispatch('setPage', option)
    }
  }
</script>

<div class="pagination-nav">
  <button
    type="button"
    class={currentPage <= 1 ? 'disabled': "active"}
    disabled="{currentPage <= 1 }"
    active="{currentPage}"
    on:click="{() => handleOptionClick(currentPage - 1)}"
  >
    <slot name="prev"></slot>
  </button>
  {#each range(lastPage, 1) as page}
    <span 
      class="{page === currentPage ? 'active': ''}"
      on:click="{() => handleOptionClick(page)}"
    >
      {page}
    </span>
  {/each}
  <button 
    type="button"  
    class={currentPage >= lastPage ? 'disabled': "active"}
    disabled="{currentPage >= lastPage}"
    active="{currentPage}"
    on:click="{() => handleOptionClick(currentPage + 1)}"
  >
    <slot name="next"></slot>
  </button>
</div>