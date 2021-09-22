<script>
  const items = ['model 3', 'safety', 'performance', 'autopilot']
</script>

<div class="absolute flex left-0 flex-col h-full justify-center">
  <ul>
    {#each items as item}
      <li class="flex flex-row place-items-center">
        <span class="w-[1px] bg-black opacity-40 h-5 ml-4 mt-[5px]" />
        <span class="ml-3 uppercase text-black text-xs font-bold mt-[5px]">
          {item}
        </span>
      </li>
    {/each}
  </ul>
</div>
