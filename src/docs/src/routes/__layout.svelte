<script>
  import { onMount } from "svelte"
  import { afterNavigate } from "$app/navigation"
  import { pagesThatDontNeedSidebar } from "@src/lib/data.js"

  import { currentLang, langs } from "@src/lib/i18n"

  onMount(() => {
    let lang = new URL(document.location).searchParams.get("lang")
    if (langs.includes(lang)) {
      $currentLang = lang
      localStorage.setItem("lang", $currentLang)
    }
    if (localStorage.getItem("lang")) {
      $currentLang = localStorage.getItem("lang")
    }
  })

  import { page } from "$app/stores"

  import "@components/StyleHandler.svelte"

  import "prism-themes/themes/prism-material-dark.css"
  import "@src/prism-themes-modify.css"

  import Navbar from "@components/Navbar.svelte"
  import Scripts from "@components/Scripts.svelte"

  import Sidebar from "@components/Sidebar.svelte"

  let drawercontent
  let drawerContentScrollY = 0
  function parseContentScroll() {
    drawerContentScrollY = drawercontent.scrollTop
  }

  let drawersidebar
  let drawerSidebarScrollY = 0
  function parseSidebarScroll() {
    drawerSidebarScrollY = drawersidebar.scrollTop
  }
  onMount(() => {
    parseContentScroll()
    parseSidebarScroll()
  })

  afterNavigate(() => {
    drawercontent.scrollTop = 0
  })

  let checked = ""
  function closeDrawer() {
    checked = ""
  }

  function openDrawer() {
    checked = true
  }

  let navbarScrollPadding = "5rem"
  function addScrollPaddingToNavbar(action) {
    navbarScrollPadding = "5rem"
  }

  function removeScrollPaddingFromNavbar(action) {
    navbarScrollPadding = "0rem"
  }
</script>

<svelte:head>
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@700&display=swap&text=daisyUIThemostpopular,freeandopen-sourceTailwindCSScomponentlibrary" rel="stylesheet" />
</svelte:head>

<div class={`bg-base-100 drawer ${pagesThatDontNeedSidebar.includes($page.url.pathname) ? "" : "drawer-mobile"}`}>
  <input id="drawer" type="checkbox" class="drawer-toggle" bind:checked />
  <div bind:this={drawercontent} on:scroll={parseContentScroll} class={`drawer-content`} style="scroll-behavior: smooth; scroll-padding-top: {navbarScrollPadding};">
    <Navbar {drawerContentScrollY} {addScrollPaddingToNavbar} {removeScrollPaddingFromNavbar} />
    <div class={`${pagesThatDontNeedSidebar.includes($page.url.pathname) ? "" : "px-6 xl:pr-2 pb-16"}`}>
      <slot />
    </div>
  </div>
  <div class="drawer-side" style="scroll-behavior: smooth; scroll-padding-top: {navbarScrollPadding};" bind:this={drawersidebar} on:scroll={parseSidebarScroll}>
    <label for="drawer" class="drawer-overlay" />
    <aside class="bg-base-200 w-80">
      <Sidebar {closeDrawer} {openDrawer} {drawerSidebarScrollY} />
      <div class="from-base-200 pointer-events-none sticky bottom-0 flex h-20 bg-gradient-to-t to-transparent" />
    </aside>
  </div>
</div>

<input type="checkbox" id="my-modal" class="modal-toggle" aria-label="Open or close modal" />
<div class="modal">
  <div class="modal-box">
    <h3 class="text-lg font-bold">Congratulations random Internet user!</h3>
    <p class="py-4">You've been selected for a chance to get one year of subscription to use Wikipedia for free!</p>
    <div class="modal-action">
      <label for="my-modal" class="btn">Yay!</label>
    </div>
  </div>
</div>

<div class="modal" id="my-modal-2">
  <div class="modal-box">
    <h3 class="text-lg font-bold">Congratulations random Internet user!</h3>
    <p class="py-4">You've been selected for a chance to get one year of subscription to use Wikipedia for free!</p>
    <div class="modal-action">
      <!-- svelte-ignore a11y-invalid-attribute -->
      <a href="#" class="btn" rel="external">Yay!</a>
    </div>
  </div>
</div>

<input type="checkbox" id="my-modal-3" class="modal-toggle" aria-label="Open or close modal" />
<div class="modal">
  <div class="modal-box relative">
    <label for="my-modal-3" class="btn btn-sm btn-circle absolute right-2 top-2">✕</label>
    <h3 class="text-lg font-bold">Congratulations random Internet user!</h3>
    <p class="py-4">You've been selected for a chance to get one year of subscription to use Wikipedia for free!</p>
  </div>
</div>

<input type="checkbox" id="my-modal-4" class="modal-toggle" aria-label="Open or close modal" />
<label for="my-modal-4" class="modal cursor-pointer">
  <label class="modal-box relative" for="">
    <h3 class="text-lg font-bold">Congratulations random Internet user!</h3>
    <p class="py-4">You've been selected for a chance to get one year of subscription to use Wikipedia for free!</p>
  </label>
</label>

<input type="checkbox" id="my-modal-5" class="modal-toggle" aria-label="Open or close modal" />
<div class="modal">
  <div class="modal-box w-11/12 max-w-5xl">
    <h3 class="text-lg font-bold">Congratulations random Internet user!</h3>
    <p class="py-4">You've been selected for a chance to get one year of subscription to use Wikipedia for free!</p>
    <div class="modal-action">
      <label for="my-modal-5" class="btn">Yay!</label>
    </div>
  </div>
</div>

<input type="checkbox" id="my-modal-6" class="modal-toggle" aria-label="Open or close modal" />
<div class="modal modal-bottom sm:modal-middle">
  <div class="modal-box">
    <h3 class="text-lg font-bold">Congratulations random Internet user!</h3>
    <p class="py-4">You've been selected for a chance to get one year of subscription to use Wikipedia for free!</p>
    <div class="modal-action">
      <label for="my-modal-6" class="btn">Yay!</label>
    </div>
  </div>
</div>

<Scripts />

<style global>
  code[class*="language-"],
  pre[class*="language-"] {
    background: unset;
  }
  .prose pre[class*="language-"] {
    max-width: 48rem;
    background-color: hsl(var(--n));
    color: hsl(var(--nc));
  }
</style>
