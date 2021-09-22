<script>
  import '../app.postcss'
  import SideNavBar from '$lib/SideNavBar.svelte'
  import TopNavBar from '$lib/TopNavBar.svelte'
</script>

<TopNavBar />
<SideNavBar />
<slot />
