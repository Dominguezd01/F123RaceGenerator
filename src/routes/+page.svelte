<script>
  import { onMount } from "svelte"
  import { gps, f1Teams, teamDriver } from "../lib/data.js"
  let gp, team, driver, performance

  const performances = ["Realitic", "Balanced"]

  const getRandomGP = async () => gps[getRandomIndex(gps)]

  const getRandomPerformance = async () =>
    performances[[getRandomIndex(performances)]]

  const getRandomTeamDriver = async () => {
    let team = f1Teams[getRandomIndex(f1Teams)]
    let driver = teamDriver.find((td) => td[team])[team][
      getRandomIndex(teamDriver.find((td) => td[team])[team])
    ]
    return { team, driver }
  }
  onMount(async () => {
    await getRandomValues()
  })

  const getRandomValues = async () => {
    gp = await getRandomGP()
    performance = await getRandomPerformance()
    let teamDriver = await getRandomTeamDriver()
    team = teamDriver.team
    driver = teamDriver.driver
  }

  const getRandomIndex = (array) => {
    return Math.floor(Math.random() * array.length)
  }
</script>

<main class="flex flex-col gap-16 items-center justify-center p-3 text-center">
  <div
    class="flex flex-row gap-16 items-center justify-center border-0 p-3 text-center h-[580px] w-[1856px]"
  >
    <div class="flex flex-col gap-5">
      <h1 class="text-6xl font-bold">GP</h1>
      {#if gp !== undefined}
        <h3 class="text-4xl">
          {gp}
        </h3>
        {#await import(`$lib/tracks/${gp}.avif`) then { default: src }}
          <img {src} class="w-[600px]" alt="Track" />
        {/await}
      {/if}
    </div>
    <div class="flex flex-col gap-5">
      <h1 class="text-6xl font-bold">Team</h1>
      {#if team !== undefined}
        <h3 class="text-4xl">
          {team}
        </h3>
        {#await import(`$lib/cars/${team}.png`) then { default: src }}
          <img {src} alt="Driver" />
        {/await}
      {/if}
    </div>
    <div class="flex flex-col gap-5">
      <h1 class="text-6xl font-bold">Driver</h1>
      {#if driver !== undefined}
        <h3 class="text-4xl">
          {driver}
        </h3>
        {#await import(`$lib/drivers/${driver}.png`) then { default: src }}
          <img {src} alt="Driver" />
        {/await}
      {/if}
    </div>
  </div>

  <div class="flex items-center">
    <button
      on:click={getRandomValues}
      class="border-2 p-3 rounded-md text-6xl text-center reload transition-all"
    >
      Reload
    </button>
  </div>
</main>
