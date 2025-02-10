<script>
    import { onMount } from "svelte";
    import { navigate } from "svelte-routing";
    import { API_URL } from "../config";
    export let id;
    let name = "";
    let image = "";

    onMount(async () => {
        if (id) {
            const response = await fetch(`${API_URL}/champions/${id}`);
            const champion = await response.json();
            name = champion.name;
            image = champion.image;
        }
    });

    async function saveChampion() {
        const method = id ? "PUT" : "POST";
        const url = id ? `${API_URL}/champions/${id}` : `${API_URL}/champions`;
        await fetch(url, {
            method,
            headers: { "Content-Type": "application/json" },
            body: JSON.stringify({ name, image })
        });
        navigate("/");
    }
</script>

<main>
    <h1>{id ? "Modifier" : "Ajouter"} un Champion</h1>
    <form on:submit|preventDefault={saveChampion}>
        <input type="text" bind:value={name} placeholder="Nom du champion" required />
        <input type="text" bind:value={image} placeholder="URL de l'image" required />
        <button type="submit">{id ? "Modifier" : "Ajouter"}</button>
    </form>
</main>

<style>
    form {
        display: flex;
        flex-direction: column;
        gap: 10px;
        margin-bottom: 20px;
    }
    button {
        background-color: blue;
        color: white;
        border: none;
        padding: 5px 10px;
        cursor: pointer;
    }
</style>
