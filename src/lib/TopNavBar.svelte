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
  import { spring } from 'svelte/motion'
  import { quintOut } from 'svelte/easing'

  let current
  let target
  let hoverBox = spring({ x: 0, diff: 0 })
  let width = writable({ text: '' })

  let visible = false

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
    visible = true
    // console.log(current)
    hoverBox.set({
      x: current.x,
      diff: diff,
    })
    width.set({
      text: `\s${e.target.id}\s`,
    })
  }

  const toggleShow = () => {
    visible = !visible
  }
</script>

{#if visible}
  <div
    id="hacked-hover"
    class="bg-gray-400 opacity-40 rounded-md absolute h-8 top-2 z-0 bg-blend-lighten text-transparent"
    transition:fade={{ duration: 100 }}
    style="transform: translateX({$hoverBox.x + 13}px)"
  >
    {$width.text}
  </div>
{/if}

<!-- style="transform: " -->
<!-- svelte-ignore a11y-mouse-events-have-key-events -->
<div
  class="fixed flex flex-row justify-center w-full"
  on:mouseenter={toggleShow}
  on:mouseleave={toggleShow}
>
  {#each items as item}
    <a
      href="/lol"
      key={item}
      id={item}
      class="py-3 px-5 text-md font-bold tracking-tighter z-20"
      on:mouseenter={(e) => {
        identifyTarget(e)
      }}
      on:mouseleave={() => {
        !visible
      }}
    >
      {item}
    </a>
  {/each}
</div>
