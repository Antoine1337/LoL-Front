<script>
    import { onMount } from "svelte";
    import { API_URL } from "../config";

    export let idJoueur;
    let playerName = ""; // Remplacez par le nom du joueur ou récupérez-le dynamiquement.
    let games = [];
    let loading = true;
    let error = null;

    onMount(async () => {
        try {
            const response = await fetch(
                `${API_URL}/players/${idJoueur}/matches`,
            );
            const responseJoueur = await fetch(
                `${API_URL}/players/${idJoueur}`,
            );
            const data = await response.json();
            const dataJoueur = await responseJoueur.json();
            playerName = dataJoueur.name
            games = data.matches; // Assurez-vous que la structure correspond à votre API.
            games = await enrichirChampions(games);
        } catch (err) {
            error = err.message;
        } finally {
            loading = false;
        }
    });
    
    const enrichirChampions = async (matches) => {
        let championsFetched = {}
        let newMatches = []
        for(const match of matches) {
            if(!championsFetched[match.champion_id]) {
                const responseChampion = await fetch(`${API_URL}/champions/${match.champion_id}`);
                championsFetched[match.champion_id] = await responseChampion.json();
            }
            match["champion"] = championsFetched[match.champion_id];
            newMatches.push(match);
        }
        return newMatches;
    }
</script>

<main>
    <h1>Parties de {playerName}</h1>

    {#if loading}
        <p>Chargement des données...</p>
    {:else if games.length === 0}
        <p>Aucune partie trouvée pour ce joueur.</p>
    {:else}
        <ul>
            {#each games as game (game.id)}
                <li class="{game.is_won ? "style-won" : "style-lost"}">
                    <div>
                        <img src={game.champion.image_url} alt={game.champion.name} />
                    </div>
                    <div>
                        <p>Joué le {game.match_date}</p>
                        <p style="font-weight: bold;">{game.is_won ? "Victoire" : "Défaite"}</p>
                    </div>
                </li>
            {/each}
        </ul>
    {/if}
</main>

<style>
    main {
        font-family: Arial, sans-serif;
        margin: 20px;
    }

    h1 {
        color: #444;
    }

    .error {
        color: red;
    }

    ul {
        list-style-type: none;
        padding: 0;
    }

    li {
        background: #f4f4f4;
        margin: 10px 0;
        padding: 10px;
        border-radius: 0.25rem;
        display: flex;
        gap: 1rem;
    }

    li div {
        display: flex;
        flex-direction: column;
        text-align: left;
        justify-content: center;
    }

    li div img {
        border-radius: 50%;
    }

    .style-won {
        background-color: #8ef591;
    }

    .style-lost {
        background-color: #f58e8e;
    }

    p {
        margin: 5px 0;
    }
</style>
