<script lang="ts">
  import "../app.css"

  import Icon from 'svelte-icon/Icon.svelte'
  import logo from '../../static/logo.svg?raw'

  import Menu from "$lib/Menu.svelte"
  import { activePageName } from "$lib/Menu.svelte"

</script>

<svelte:head>
	<title>lancache-ui/{ $activePageName }</title>
</svelte:head>

<header>

  <div class="h-full">
    <!-- Static sidebar for desktop -->
    <div class="hidden md:flex md:w-64 md:flex-col md:fixed md:inset-y-0">

      <!-- Sidebar component, swap this element with another sidebar if you like -->
      <div class="flex-1 flex flex-col min-h-0 bg-gray-800">
        <div class="flex-1 flex flex-col pt-5 pb-4 overflow-y-auto">
          <div class="flex items-center flex-shrink-0 px-4">
            <!-- Logo Icon made from <a href="http://www.onlinewebfonts.com/icon">Icon Fonts</a> is licensed by CC BY 3.0 -->
            <Icon data={ logo } size="40px" stroke="white" />
            <div class="text-2xl ml-7">lancache-ui</div>
          </div>
          <Menu />
        </div>

        <!-- Begin Donate component -->
        <div class="flex-shrink-0 flex bg-gray-700 p-4">
          <a href="#donate" alt="donate" class="flex-shrink-0 w-full group block">
            <div class="flex items-center">
              <div>
                <img class="inline-block h-9 w-9 rounded-full" src="donate-kofi.svg" alt="">
              </div>
              <div class="ml-3">
                <p class="text-xs font-medium text-gray-300 group-hover:text-gray-200">
                  Made with <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" class="w-6 h-6">
                    <path d="M11.645 20.91l-.007-.003-.022-.012a15.247 15.247 0 01-.383-.218 25.18 25.18 0 01-4.244-3.17C4.688 15.36 2.25 12.174 2.25 8.25 2.25 5.322 4.714 3 7.688 3A5.5 5.5 0 0112 5.052 5.5 5.5 0 0116.313 3c2.973 0 5.437 2.322 5.437 5.25 0 3.925-2.438 7.111-4.739 9.256a25.175 25.175 0 01-4.244 3.17 15.247 15.247 0 01-.383.219l-.022.012-.007.004-.003.001a.752.752 0 01-.704 0l-.003-.001z" />
                  </svg>
                </p>
                <p class="text-xs font-medium text-gray-300 group-hover:text-gray-200">
                  Donate via Ko-Fi
                </p>
              </div>
            </div>
          </a>
        </div>
        <!-- End Donate component -->

      </div>
    </div>

    <!-- Begin Sidebar for Mobile -->
    <div class="md:pl-64 flex flex-col flex-1">
      <div class="sticky top-0 z-10 md:hidden pl-1 pt-1 sm:pl-3 sm:pt-3 bg-gray-100">
        <button type="button" class="-ml-0.5 -mt-0.5 h-12 w-12 inline-flex items-center justify-center rounded-md text-gray-500 hover:text-gray-900 focus:outline-none focus:ring-2 focus:ring-inset focus:ring-indigo-500">
          <span class="sr-only">Open Menu</span>
          <!-- Heroicon name: outline/menu -->
          <svg class="h-6 w-6" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" aria-hidden="true">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16" />
          </svg>
        </button>
      </div>

      <main class="flex-1">
        <div class="py-6">
          <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <h1 class="text-2xl font-semibold text-gray-900">{ $activePageName }</h1>
          </div>

          <br>

          <div class="max-w-7xl mx-auto px-4 sm:px-6 md:px-8 bg-gray-100 rounded-lg">

            <!-- Begin Page Content -->
            <div class="py-4">

              <!--
              <div class="h-96 rounded-lg border-4 border-dashed border-gray-200">
                <slot />
              </div>
              -->

              <slot />

            </div>
            <!-- /End Page Content -->

          </div>
        </div>
      </main>
    </div>
    <!-- End Sidebar for Mobile -->

  </div>

</header>
