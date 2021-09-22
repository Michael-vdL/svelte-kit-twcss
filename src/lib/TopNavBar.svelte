<!-- 
  floaty hover thing set to invisible
  absolute and lower z index

  removed when mouse leave parent div
  spawned when mouse enter element
  position changed when mouse move to new div

  to determin location:
    - when mouse on div - mark target
    - get target div computed location (how?)
    - translate div location by that distance
 -->
<script>
  import { onMount } from 'svelte'
  import { writable } from 'svelte/store'
  import { fly, fade } from 'svelte/transition'
  import { quintOut } from 'svelte/easing'

  let current
  let target
  let hoverBox = writable({ x: 0, text: '', diff: 0 })

  let needsUpdate = false

  const items = [
    'Model S',
    'Model 3',
    'Model X',
    'Model Y',
    'Solar Roof',
    'Solar Panels',
  ]

  const identifyTarget = (e) => {
    const element = document.getElementById(e.target.id)
    let diff = 0
    if (!current) {
      current = element.getBoundingClientRect()
    } else {
      target = element.getBoundingClientRect()
      diff = target.x - current.x
      current = target
    }
    // console.log(current)
    hoverBox.set({
      x: current.x,
      diff: diff,
      text: `A${e.target.id}A`,
    })
    needsUpdate = true
    // hoverBox.set({
    //   // x: cur,
    // })
  }
</script>

{#if needsUpdate}
  <div
    id="hacked-hover"
    class="bg-gray-400 opacity-40 rounded-md absolute h-8 top-2 z-0 bg-blend-lighten text-transparent transform"
    in:fly={{ x: -$hoverBox.diff, duration: 300 }}
    out:fade={{ duration: 10000 }}
    style="transform: translateX({$hoverBox.x + 10}px)"
  >
    {$hoverBox.text}
  </div>
{/if}

<!-- style="transform: " -->
<div class="fixed flex flex-row justify-center w-full">
  {#each items as item}
    <div
      key={item}
      id={item}
      class="py-3 px-5 text-md font-bold tracking-tighter z-20"
      on:mouseenter={(e) => identifyTarget(e)}
      on:mouseleave={() => {
        needsUpdate = false
      }}
    >
      {item}
    </div>
  {/each}
  <!-- <svg class="absolute w-full h-full">
    <rect
      class="fill-current text-gray-400 opacity-25 bg-blend-lighten"
      x={$hoverBox.x}
      y={8}
      width={$hoverBox.w}
      height={32}
      rx={10}
    />
  </svg> -->
</div>
<!-- on:mousemove={(e) => hoverBox.set({ x: e.clientX, y: e.clientY })} -->
