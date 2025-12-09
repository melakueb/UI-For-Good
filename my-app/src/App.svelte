<script>
  import Blog from "./Blog.svelte";        
  import Profile from "./Profile.svelte";  
  import Upskill from "./Upskill.svelte";
  import Jobs from "./Jobs.svelte";

  let currentView = "blog";

  function go(view) {
    currentView = view;
  }
</script>

<div class="app">
  <!-- Top nav -->
  <header class="top-nav">
    <div class="nav-left">
      <img class="logo-img" src="/images/lotuslogo.png" alt="Lotus logo" />

      <div class="brand">
        <span class="brand-title">Lotus</span>
        <span class="brand-subtitle">Bloom Anew</span>
      </div>
    </div>

    <nav class="nav-tabs">
      <button
        class:selected={currentView === "blog"}
        on:click={() => go("blog")}
      >
        Profiles
      </button>
      <button
        class:selected={currentView === "profile"}
        on:click={() => go("profile")}
      >
        My Profile
      </button>
      <button
        class:selected={currentView === "upskill"}
        on:click={() => go("upskill")}
      >
        Upskill
      </button>
      <button
        class:selected={currentView === "jobs"}
        on:click={() => go("jobs")}
      >
        Jobs
      </button>
    </nav>
  </header>

  <main class="main-shell">
    {#if currentView === "blog"}
      <Blog />
    {:else if currentView === "profile"}
      <Profile />
    {:else if currentView === "upskill"}
      <Upskill />
    {:else}
      <Jobs />
    {/if}
  </main>
</div>
