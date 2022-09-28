<script context="module" lang="ts">
  //export const prerender = true
</script>

<script lang="ts">
  import Dashboard from "./dashboard/+page.svelte"
</script>

<svelte:head>
	<title>Dashboard</title>
</svelte:head>

{#if false}<slot></slot>{/if}
<Dashboard />
