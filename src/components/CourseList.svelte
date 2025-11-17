<script>
  import { createEventDispatcher } from 'svelte';
  const dispatcher = createEventDispatcher();

  export let courses = [];

  function removeCourse(id) {
    dispatcher('deleteCourse', id);
  }
</script>

{#if courses.length === 0}
  <p class="empty">No classes yet. Add one on the left.</p>
{:else}
  <ul class="course-list">
    {#each courses as c}
      <li>
        <div class="info">
          <div class="title">{c.name}</div>
          <div class="meta">
            <span>{c.location || 'Location TBA'}</span>
            <span>• {c.days.join(', ')}</span>
            <span>• {c.startTime}–{c.endTime}</span>
          </div>
        </div>
        <button class="delete" on:click={() => removeCourse(c.id)}>✕</button>
      </li>
    {/each}
  </ul>
{/if}

<style>
  .course-list {
    list-style: none;
    padding: 0;
    margin: 0;
    display: flex;
    flex-direction: column;
    gap: 0.4rem;
  }

  .course-list li {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 0.45rem 0.6rem;
    border-radius: 0.5rem;
    background: #f5f5f5;
  }

  .title {
    font-weight: 600;
    font-size: 0.9rem;
  }

  .meta {
    display: flex;
    gap: 0.35rem;
    font-size: 0.8rem;
    color: #555;
    flex-wrap: wrap;
  }

  .delete {
    border: none;
    border-radius: 999px;
    padding: 0.2rem 0.5rem;
    cursor: pointer;
    background: #ef4444;
    color: white;
    font-size: 0.8rem;
  }

  .delete:hover {
    opacity: 0.9;
  }

  .empty {
    font-size: 0.9rem;
    color: #777;
  }
</style>
