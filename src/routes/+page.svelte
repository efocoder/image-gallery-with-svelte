<script lang="ts">
    import axios from 'axios'
    import {onMount} from "svelte";
    import  {fade, fly} from "svelte/transition"

    let term = ""
    let photos: {id: string, alt_description: string, urls: {regular: string}}[] = []


    onMount(() => {
        fetchData();
    })
    const fetchData = async () => {
        const response = await axios.get(
            `https://api.unsplash.com/search/photos?page=1&query=${term || 'office'}&client_id=RaKoM04e3WpHtOyorNrfR_iOFeemoui5cNlMGJl56sI`);

        photos = response.data.results;
    }

    const handleSearch =  async () => {
        if (!term) return;
        await fetchData();
        term = "";
    }


</script>

<div class="container">
    <div class="header">
        <h1>Image Gallery </h1>
        <div class="input-container">
            <input type="text" class="input" bind:value={term} />
            <button class="button" on:click={handleSearch}>Search</button>
        </div>
    </div>
    <div class="photos">
        {#each photos as photo, i (photo.id) }
            <img src={photo.urls.regular} alt={photo.alt_description} class="image"
                 in:fly={{
                        y: 200,
                        duration: 2000,
                        delay: i * 200
                    }}
                 out:fade
            >
        {/each}
    </div>
</div>

<style>
    .image {
        width: 400px;
        height: 250px;
        margin: 5px;
    }
    .photos {
        display: flex;
        flex-wrap: wrap;
    }
    .container {
        width: 1230px;
        margin: 0 auto;
    }
    .header {
        text-align: center;
        font-size: 20px;
    }
    .input {
        padding: 10px;
        width: 400px;
        border-radius: 10px;
        outline: none;
        border: 1px solid gray;
        font-size: 20px;
    }
    .button {
        padding: 10px;
        font-size: 20px;
        background-color: aqua;
        border-radius: 10px;
        border: none;
        color: white;
    }
    .input-container {
        margin-bottom: 40px;
    }
</style>

