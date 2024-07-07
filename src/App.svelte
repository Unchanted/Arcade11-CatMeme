<script>
  import { onMount } from 'svelte';

  let catImage = '';
  let topText = '';
  let bottomText = '';

  const catApiUrl = 'https://api.thecatapi.com/v1/images/search';
  const texts = [
    "I fits, I sits",
    "Human, I require food",
    "Why you no love me?",
    "I'm not fat, I'm fluffy",
    "Feed me meow",
    "I am the night",
    "Nap time is all the time",
    "I regret nothing",
    "Purr-fection",
    "Cat hair, don't care"
  ];

  function getRandomText() {
    return texts[Math.floor(Math.random() * texts.length)];
  }

  async function generateMeme() {
    try {
      const response = await fetch(catApiUrl);
      const data = await response.json();
      catImage = data[0].url;
      topText = getRandomText();
      bottomText = getRandomText();
    } catch (error) {
      console.error('Error fetching cat image:', error);
    }
  }

  onMount(() => {
    generateMeme();
  });
</script>

<main>
  <h1>Random Cat Meme Generator</h1>
  <div class="meme-container">
    {#if catImage}
      <div class="meme">
        <div class="meme-text top-text">{topText}</div>
        <img src={catImage} alt="Random cat" />
        <div class="meme-text bottom-text">{bottomText}</div>
      </div>
    {:else}
      <p>Loading...</p>
    {/if}
  </div>
  <button on:click={generateMeme}>Generate New Meme</button>
</main>

<style>
  main {
    text-align: center;
    padding: 20px;
    font-family: Arial, sans-serif;
  }

  .meme-container {
    max-width: 500px;
    margin: 0 auto;
    position: relative;
  }

  .meme {
    position: relative;
    margin-bottom: 20px;
  }

  img {
    width: 100%;
    height: auto;
  }

  .meme-text {
    position: absolute;
    left: 50%;
    transform: translateX(-50%);
    color: white;
    font-size: 24px;
    font-weight: bold;
    text-shadow: 2px 2px 4px #000000;
    width: 100%;
  }

  .top-text {
    top: 10px;
  }

  .bottom-text {
    bottom: 10px;
  }

  button {
    font-size: 18px;
    padding: 10px 20px;
    cursor: pointer;
  }
</style>
