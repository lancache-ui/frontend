<script>
  export let game;
  export let id;

  if (!game) {
    game = {}
  }

  // TODO: header needs to be on server, because it could come from Steam/Epic, etc
  $: headerURL = `https://cdn.cloudflare.steamstatic.com/steam/apps/${id}/header.jpg`

</script>

<li class="relative bg-black focus-within:ring-2 focus-within:ring-inset focus-within:ring-indigo-600 hover:bg-gray-50">

  <div
  style="background:
    linear-gradient(
      to bottom,
      rgba(0, 0, 0, 0.9),
      rgba(0, 0, 0, 0)
    ),
    url('{headerURL}');
    background-size: cover;
    background-repeat: no-repeat;
    "
    class="flex-row bg-local bg-auto bg-no-repeat bg-center select-none py-5 px-4 bg-black"
  >

    <div class="flex justify-between space-x-3">
      <div class="min-w-0 flex-1">
        <a href="game/{ 0 }" class="block focus:outline-none" alt="Game Name">
          <span class="absolute inset-0" aria-hidden="true"></span>
          <p class="truncate text-m font-medium text-white-900">{ game?.name || 'Unknown' }</p>
          <p class="truncate text-sm text-white-500">{ game?.content || 'Content' }</p>
        </a>
      </div>
    </div>

    <div class="h-20 pt-16 w-4">
      <div class="bg-gray-50"><center>3</center></div>
    </div>

    <!-- TODO: add provider button (Steam, Epic, etc)-->
    <!-- Show clients icon (little users icon with a number), this applies to top/largest by size to quickly show how many people interested -->
  </div>
</li>

<style>
</style>
