<script lang="ts">
  import axios from 'axios';
	import { onMount } from 'svelte';
  import {fade, fly} from 'svelte/transition';
   
  let term='';
  let photos: {
    id:string;
    alt_description: string;
    urls: {regular:string}
  }[] = []

  const fetchData = async () => {
    const response = await axios.get(`https://api.unsplash.com/search/photos?page=1&query=${term || 'office'}&client_id=Ic0BIJJ_iiVZ2Bv9wyuXcDV9399tj3eJhu1fS6ziZGk`);
    //state
    photos = response.data.results;
  }

  onMount(()=> {
    fetchData();
  })


  console.log({term, photos});

  const handleSearch= async ()=> {
    if(!term) return;
    await fetchData();
    term = '';
  }

</script>

<div class="container">
  <div class="header">
    <h1>Image Gallery</h1>
    <div class="input-container">
      <!-- binding: input is changing the state value of term -->
      {term}
      <input type="text" class="input" bind:value={term}>
      <button on:click={()=> handleSearch()} class="button">Search</button>
    </div>
  </div>
  <div class="photos">
    {#each photos as photo, i (photo.id)}
    <img class='image' src={photo.urls.regular} alt={photo.alt_description} in:fly={{y:200, duration: 2000, delay: i * 200}} out:fade>{/each}
  </div>
</div>


<style>
  .image {
    width: 300px;
    height: 200px;
    margin: 5px;
  }
  .photos {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
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
    cursor: pointer;
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