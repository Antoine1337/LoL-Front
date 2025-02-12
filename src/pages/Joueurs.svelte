<script>
    import { onMount } from "svelte";
    import { API_URL } from "../config.js";
    import { navigate } from "svelte-routing";

    let players = [];

    // Fonction pour récupérer les joueurs depuis l'API
    const fetchPlayers = async () => {
        const response = await fetch(`${API_URL}/players`);
        players = await response.json();
        players = players['players'];
    };

    // Charger les joueurs au montage du composant
    onMount(fetchPlayers);

    // Fonction pour rediriger vers la page de l'historique d'un joueur
    const showPlayerHistory = (playerId) => {
        navigate(`/players/${playerId}/matches`);
    };
</script>

<main>
    <h1>Liste des joueurs</h1>
    {#if players.length > 0}
        <ul>
            {#each players as player}
                <li>
                    <button on:click={() => showPlayerHistory(player.id)}>
                        <div>
                            {player.name}
                        </div>
                        <div>
                            {player.division}
                        </div>
                    </button>
                </li>
            {/each}
        </ul>
    {:else}
        <p>Aucun joueur trouvé.</p>
    {/if}
</main>

<style>
    /* Style minimal pour la page */
    main {
        padding: 20px;
    }
    button {
        font-size: 1.2em;
        margin: 5px;
        padding: 10px;
        cursor: pointer;
    }

    ul {
        display: flex;
        flex-direction: column;
        text-align: left;
    }

    li {
        list-style: none;
    }

    li button {
        display: flex;
        justify-content: space-between;
        min-width: 350px;
    }
</style>
