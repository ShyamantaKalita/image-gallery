<script lang="ts">
    import axios from "axios";
    import { onMount } from "svelte";
    import { fade, fly, blur } from "svelte/transition";

    let term: String = "";
    let photos: {
        id: string;
        alt_description: string;
        urls: { regular: string };
    }[] = [];
    const fetchData = async () => {
        const response = await axios.get(
            `https://api.unsplash.com/search/photos?page=1&query=${
                term || "Office"
            }&client_id=I9CdkeZpwIarFpM_uUaY7MmFuj0IBiRNVX8SbodfBSo`
        );

        photos = response.data.results;
        //console.log(photos);
    };

    onMount(() => {
        fetchData();
        console.log(photos);
    });

    async function handleSearch() {
        if (!term) return;
        await fetchData();
        term = "";
    }
</script>

<div class="container">
    <div class="header">
        <h1>Image Gallery</h1>
        <div class="input-container">
            <input type="text" class="input" bind:value={term} />
            <button class="button" on:click={handleSearch}>Search</button>
        </div>
    </div>
    <div class="photos">
        {#each photos as photo, i (photo.id)}
            <img
                src={photo.urls.regular}
                alt={photo.alt_description}
                class="image"
                in:fly={{ y: 200, duration: 2000, delay: i * 200 }}
            />
        {/each}
    </div>
</div>

<!-- https://api.unsplash.com/search/photos?page=1&query=office&client_id=I9CdkeZpwIarFpM_uUaY7MmFuj0IBiRNVX8SbodfBSo
search/photos?page=1&query=office -->

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
