<script context="module">
  import { writable } from 'svelte/store'
  export const activePageName = writable('Dashboard')

  export const menuItems = [
  {
    name: 'Dashboard',
    path: '/',
    icon: '',
  },
  {
    name: 'Active',
    path: '/active',
  },
  {
    name: 'History',
    path: '/history',
  },
  {
    name: 'Stats',
    path: '/stats',
  },
  {
    name: 'Users',
    path: '/users',
  },
  {
    name: 'Settings',
    path: '/settings',
  },
  {
    name: 'Credits',
    path: '/credits',
    display: false,
  },
]
</script>

<script lang="ts">
  import { page } from '$app/stores'

  function menuFromPage(page: string) {
    for (let menuItem of menuItems) {
      if (menuItem.path === page)
        return menuItem.name
    }

    return ''
  }

  // TODO: look into using Svelte:head for this
  $: activePage = $page.url.pathname.toLocaleLowerCase()
  $: activePageName.set(menuFromPage(activePage))

</script>

<svelte:options immutable={true}/>

<section>
  <nav class="mt-5 flex-1 px-2 space-y-1">

    <!-- sveltekit:prefetch -->

    {#each menuItems as menuItem}

    {#if menuItem.display !== false}
      <a
        class:h-full={ menuItem.path === activePage }
        class:bg-gray-900={ menuItem.path === activePage }
        class:text-white={ menuItem.path === activePage }

        href={ menuItem.path }

        class="text-gray-300 hover:bg-gray-700 hover:text-white group flex items-center px-2 py-2 text-sm font-medium rounded-md">

        <!-- TODO: iconForPage -->
        <!-- Hero Icons provided by: https://heroicons.com/ -->

        {#if menuItem.name === 'Dashboard' }
          <svg class="text-gray-400 group-hover:text-gray-300 mr-3 flex-shrink-0 h-6 w-6" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 12l2-2m0 0l7-7 7 7M5 10v10a1 1 0 001 1h3m10-11l2 2m-2-2v10a1 1 0 01-1 1h-3m-6 0a1 1 0 001-1v-4a1 1 0 011-1h2a1 1 0 011 1v4a1 1 0 001 1m-6 0h6" />
          </svg>
        {:else if menuItem.name === 'Settings'}
          <svg class="text-gray-400 group-hover:text-gray-300 mr-3 flex-shrink-0 h-6 w-6" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10.325 4.317c.426-1.756 2.924-1.756 3.35 0a1.724 1.724 0 002.573 1.066c1.543-.94 3.31.826 2.37 2.37a1.724 1.724 0 001.065 2.572c1.756.426 1.756 2.924 0 3.35a1.724 1.724 0 00-1.066 2.573c.94 1.543-.826 3.31-2.37 2.37a1.724 1.724 0 00-2.572 1.065c-.426 1.756-2.924 1.756-3.35 0a1.724 1.724 0 00-2.573-1.066c-1.543.94-3.31-.826-2.37-2.37a1.724 1.724 0 00-1.065-2.572c-1.756-.426-1.756-2.924 0-3.35a1.724 1.724 0 001.066-2.573c-.94-1.543.826-3.31 2.37-2.37.996.608 2.296.07 2.572-1.065z" />
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 12a3 3 0 11-6 0 3 3 0 016 0z" />
          </svg>
        {:else if menuItem.name === 'Active' }
          <svg class="text-gray-400 group-hover:text-gray-300 mr-3 flex-shrink-0 h-6 w-6" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 7h8m0 0v8m0-8l-8 8-4-4-6 6" />
          </svg>
        {:else if menuItem.name === 'History' }
          <svg class="text-gray-400 group-hover:text-gray-300 mr-3 flex-shrink-0 h-6 w-6" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 13v-1m4 1v-3m4 3V8M8 21l4-4 4 4M3 4h18M4 4h16v12a1 1 0 01-1 1H5a1 1 0 01-1-1V4z" />
          </svg>
        {:else if  menuItem.name === 'Stats' }
          <svg class="text-gray-400 group-hover:text-gray-300 mr-3 flex-shrink-0 h-6 w-6" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 11H5m14 0a2 2 0 012 2v6a2 2 0 01-2 2H5a2 2 0 01-2-2v-6a2 2 0 012-2m14 0V9a2 2 0 00-2-2M5 11V9a2 2 0 012-2m0 0V5a2 2 0 012-2h6a2 2 0 012 2v2M7 7h10" />
          </svg>
        {:else if menuItem.name === 'Users' }
          <svg class="text-gray-400 group-hover:text-gray-300 mr-3 flex-shrink-0 h-6 w-6" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" aria-hidden="true">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 4.354a4 4 0 110 5.292M15 21H3v-1a6 6 0 0112 0v1zm0 0h6v-1a6 6 0 00-9-5.197M13 7a4 4 0 11-8 0 4 4 0 018 0z" />
          </svg>
        {/if}

        { menuItem.name }
      </a>
      {/if}

    {/each}

  </nav>
</section>

<div class="grid grid-cols-1 divide-y divide-gray-100">
  <br>
  <div></div>
  <section>
    <nav class="mt-5 flex-1 px-2 space-y-1">
      <a
        class:h-full={ "Credits" === activePage }
        class:bg-gray-900={ "Credits" === activePage }
        class:text-white={ "Credits" === activePage }

        href="credits"

        class="text-gray-300 hover:bg-gray-700 hover:text-white group flex items-center px-2 py-2 text-sm font-medium rounded-md">

        <svg class="text-gray-400 group-hover:text-gray-300 mr-3 flex-shrink-0 h-6 w-6" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" aria-hidden="true">
          <path stroke-linecap="round" stroke-linejoin="round" d="M12 7.5h1.5m-1.5 3h1.5m-7.5 3h7.5m-7.5 3h7.5m3-9h3.375c.621 0 1.125.504 1.125 1.125V18a2.25 2.25 0 01-2.25 2.25M16.5 7.5V18a2.25 2.25 0 002.25 2.25M16.5 7.5V4.875c0-.621-.504-1.125-1.125-1.125H4.125C3.504 3.75 3 4.254 3 4.875V18a2.25 2.25 0 002.25 2.25h13.5M6 7.5h3v3H6v-3z" />
        </svg>

        Project Credits
      </a>
    </nav>
  </section>
</div>
