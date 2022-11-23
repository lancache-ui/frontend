<script lang="ts">
  import GameCard from "./gameCard.svelte";

  import { onMount } from 'svelte'
  import { writable } from 'svelte/store';

  let totalReqs = writable(0)
  let totalHits = writable(0)
  let totalMisses = writable(0)

  let activeGames = writable([])

  function gql(query: string, cb: Function) {
    const url = new URL('http://100.123.231.77:3000/api')

    url.searchParams.append(
      'query',
      query,
    )

    const eventsource = new EventSource(url.href, {
      withCredentials: true, // This is required for cookies
    })

    eventsource.onmessage = (event) => {
      const eData = JSON.parse(event.data)
      const data = eData.data
      cb(data)
    }
  }

  function parseStats(stats: { totalReqs: any; totalHits: number; totalMisses: number; }) {
    console.log('Parsing stats:')
    console.log(stats)

    totalReqs.set(stats.totalReqs.toLocaleString('en-US'))
    addGlow()

    // TODO: consider using `.toFixed(2) for the code below, as pointed out by Discord user `pingger`
    // https://stackoverflow.com/questions/661562/how-to-format-a-float-in-javascript/661579#661579
    // https://stackoverflow.com/questions/566564/math-roundnum-vs-num-tofixed0-and-browser-inconsistencies
    // After researching just a tad, Math.round appears to be more reliable across browsers.

    // Nothing to compute.
    if (!stats.totalReqs || stats.totalReqs < 1) {
      totalHits.set(0)
      totalMisses.set(0)
      return
    }

    const totalHitRatio = Math.round( (stats.totalHits / stats.totalReqs) * 100 )
    const totalMissRatio = Math.round( (stats.totalMisses / stats.totalReqs) * 100 )

    totalHits.set(totalHitRatio)
    totalMisses.set(totalMissRatio)
  }

  function subStats() {
    gql(
      /* GraphQL */ `
        subscription {
          stats {
            totalReqs
            totalHits
            totalMisses
          }
        }
      `, (data: { stats: any; }) => {
        const stats = data.stats
        parseStats(stats)
      })
  }

  function subActiveGames() {
    gql(
      /* GraphQL */ `
        subscription {
          active {
            id
            service
            name
            content
          }
        }
      `, (data: { active: any; }) => {
        const games = data.active
        //$activeGames = games
        activeGames.set(games)
      })
  }

  onMount(async () => {

    // Hydrate page info before subscriptions take over.
    const url = new URL('http://100.123.231.77:3000/api')

    url.searchParams.append(
      'query',
      /* GraphQL */ `
        query {
          stats {
            totalHits
            totalMisses
            totalReqs
          }
          active {
            id
            service
            name
            content
          }
        }
      `,
    )

    const res = await fetch(url.toString())
    const fData = await res.json()
    const data = fData.data

    const stats = data.stats
    const games = data.active

    parseStats(stats)
    activeGames.set(games)

    subStats()
    subActiveGames()
  })



  function addGlow() {
    const glower = document.getElementById('glower')
    glower?.classList.add("glow")
    setTimeout(() => glower?.classList.remove("glow"), 1000)
  }

  const topGames = [
    {
      id: 860950,
      name: 'Mark of the Ninja: Remastered',
      content: 'Mark of the Ninja Remastered Content - Linux',
      //header: 'https://cdn.cloudflare.steamstatic.com/steam/apps/860950/header.jpg?t=1663024204',
    },
    {
      id: 322330,
      name: 'Don\'t Starve Together',
      content: 'Don\'t Starve Together - Windows',
    },
    {
      id: 15750,
      name: 'Oddworld: Stranger\'s Wrath HD',
      content: 'Stranger-Depot',
      //header: 'https://cdn.cloudflare.steamstatic.com/steam/apps/15750/header.jpg?t=1663421409',
    }
  ]
</script>

<section class="h-full">
  <div class="h-full">
    <h3 class="text-lg leading-6 font-medium text-gray-900">

      <div>
        <!-- Preload CSS for js classes to work -->
        <div style="visibility: hidden;" class="glow" />

        <dl class="mt-5 grid grid-cols-1 gap-5 sm:grid-cols-3">
          <div class="overflow-hidden rounded-lg bg-white px-4 py-5 shadow sm:p-6 bg-green">
            <dt class="truncate text-sm font-medium text-white">Total Requests (2 clients)</dt>
            <dd id="glower" class="mt-1 text-3xl font-semibold tracking-tight text-white">{ $totalReqs }</dd>
          </div>

          <div class="overflow-hidden rounded-lg bg-white px-4 py-5 shadow sm:p-6 bg-aqua">
            <dt class="truncate text-sm font-medium text-white">Cache Hits</dt>
            <dd class="mt-1 text-3xl font-semibold tracking-tight text-gray-900">{ $totalHits }%</dd>
          </div>

          <div class="overflow-hidden rounded-lg bg-white px-4 py-5 shadow sm:p-6 bg-red">
            <dt class="truncate text-sm font-medium text-white">Cache Misses</dt>
            <dd class="mt-1 text-3xl font-semibold tracking-tight text-gray-900">{ $totalMisses }%</dd>
          </div>
        </dl>

        <!--
        <br>
        <h1 on:click={addGlow}>Test</h1>
        -->
      </div>

      <!-- stats/banner.svelte -->
      <!-- active/small.svelte -->

      <!--
        TODO:
        - active games
        - top games - most hits by children depots (as long as child isn't MULTI)
        - largest games (by depot size)
        - //most saved bandwidth games (implied by top games)
      -->

      <!-- <div class="flex-grid" style="display: flex;"> -->
      <!-- <div class="flex flex-row space-x-2 justify-evenly"> -->

        <div class="mt-5 grid grid-cols-1 gap-5 sm:grid-cols-3">
        <!-- <div class="col p4" style="flex: 1;"> -->


          <ul class="divide-y divide-gray-200">
            <center>Active</center>

            <!-- TODO: Empty card for No Games Active -->
            {#each $activeGames as game }
              <GameCard id={ game.id } game={ game }/>
            {/each}

          </ul>

          <ul class="divide-y divide-gray-200">
            <center>Top</center>

            {#each topGames as game }
              <GameCard id={ game.id } game={ game }/>
            {/each}
          </ul>

          <ul class="divide-y divide-gray-200 p1">
            <center>Largest by Size</center>

            <GameCard id={ 0 } game={{}} />
          </ul>

      </div>


    </h3>

  </div>

</section>

<style>
  .bg-green {
    background-color: rgb(0, 74, 40) !important;
  }

  .bg-aqua {
    background-color: #007997 !important;
  }

  /*.bg-yellow {
    background-color: #b1720c !important;
  }*/

  .bg-red {
    background-color: #913225 !important;
  }

  .glow {
    animation-name: Pulse;
    animation-duration: 1s;
  }

  @keyframes Pulse {
    from {
      text-shadow: 0 0 0px currentColor;
    }

    50% {
      text-shadow: 0 0 5px currentColor;
    }

    to {
      text-shadow: 0 0 0px currentColor;
    }
  }
</style>
