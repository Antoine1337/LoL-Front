<script>
    import { Router, Route, Link } from "svelte-routing";
    import Home from "./pages/Home.svelte";
    import ChampionForm from "./pages/ChampionForm.svelte";
    import Joueurs from "./pages/Joueurs.svelte";
    import JoueursMatches from "./pages/JoueursMatches.svelte";
    export let url = "";
</script>

<main>
    <Router {url}>
        <nav>
            <Link to="/"><div class="nav-item">Champions</div></Link>
            <Link to="/champion-form"><div class="nav-item">Gérer Champions</div></Link>
            <Link to="/players"><div class="nav-item">Joueurs</div></Link>
        </nav>
        <div>
            <Route path="/"><Home /></Route>
            <Route path="/champion-form/:id" let:params>
                <ChampionForm id={params.id} />
            </Route>
            <Route path="/champion-form" component={ChampionForm} />
            <Route path="/players" component={Joueurs} />
            <Route path="/players/:id/matches" let:params>
                <JoueursMatches idJoueur={params.id} />
            </Route>
        </div>
    </Router>
</main>

<style>
    nav {
        display: flex;
        gap: 10px;
        margin-bottom: 20px;
    }

    .nav-item {
        background-color: whitesmoke;
        padding: 0.5rem 1rem;
        border-radius: 0.25rem;
        cursor: pointer;
        border: 1px solid transparent;
        transition: border-color 0.25s;
    }

    .nav-item:hover {
        border-color: #646cff;
    }
</style>
