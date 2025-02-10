<script>
  import { Router, Route } from "svelte-routing";
  import Home from "./pages/Home.svelte";
  import ChampionForm from "./pages/ChampionForm.svelte";
  // import Players from "./pages/Players.svelte";
</script>

<main>
  <nav>
    <a href="/">Champions</a>
    <a href="/champion-form">Gérer Champions</a>
    <a href="/players">Joueurs</a>
  </nav>
  <Router>
    <Route path="/" component={Home} />
    <Route path="/champion-form" component={ChampionForm} />
    <!-- <Route path="/players" component={Players} /> -->
  </Router>
</main>

<style>
  nav {
    display: flex;
    gap: 10px;
    margin-bottom: 20px;
  }
</style>
