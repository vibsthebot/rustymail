<script lang="ts">
  import { invoke } from '@tauri-apps/api';
  import Email from './Email.svelte';
  let unreadEmails = invoke<number[]>("fetch_unread_uids");
  //let unreadEmails = [1, 2];
  console.log(unreadEmails);
</script>

{#await unreadEmails}
  <p>Loading emails...</p>
{:then uids}
  {#each uids as uid}
    <Email {uid}/>
  {/each}
{:catch error}
  <p class="error">Error loading emails: {error.message}</p>
{/await}