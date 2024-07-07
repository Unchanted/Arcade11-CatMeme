<script>
  import { onMount } from 'svelte';
  import BackgroundGradient from './BackgroundGradient.svelte';

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
    "Cat hair, don't care",
    "Bow before your feline overlord",
    "Coffee right meow",
    "I'm not lazy, I'm energy efficient",
    "Mondays... why?",
    "I'm not antisocial, I just like my space",
    "You had me at meow",
    "I'm not bossy, I have leadership skills",
    "I'm not grumpy, this is just my face",
    "Catnip: not even once",
    "Did someone say treats?",
    "I don't need Google, I'm already a know-it-all",
    "I'm not arguing, I'm just explaining why I'm right",
    "My bed, my rules",
    "I'm not ignoring you, I'm just busy judging you",
    "Pawsome day ahead"
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
      <BackgroundGradient
        containerClassName="background-gradient-container"
        className="background-gradient"
        colors={["#0e0e0e", "#cf1e1e", "#ff2e2e"]}
      >
        <div class="meme">
          <div class="meme-text top-text">{topText}</div>
          <img src={catImage} alt="Random cat" />
          <div class="meme-text bottom-text">{bottomText}</div>
        </div>
      </BackgroundGradient>
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
    display: flex;
    flex-direction: column;
    align-items: center;
    max-width: 800px;
    margin: 0 auto;
  }

  .meme-container {
    width: 100%;
    max-width: 500px;
    margin: 0 auto 20px;
    position: relative;
  }

  .meme {
    position: relative;
    padding: 20px;
  }

  img {
    width: 100%;
    height: auto;
    display: block;
    border-radius: 10px;
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
    padding: 0 10px;
    box-sizing: border-box;
  }

  .top-text {
    top: 30px;
  }

  .bottom-text {
    bottom: 30px;
  }

  button {
    font-size: 18px;
    padding: 10px 20px;
    cursor: pointer;
    background-color: #4CAF50;
    color: white;
    border: none;
    border-radius: 5px;
    transition: background-color 0.3s;
  }

  button:hover {
    background-color: #45a049;
  }

  :global(.background-gradient-container) {
    border-radius: 20px;
    padding: 20px;
  }

  :global(.background-gradient) {
    border-radius: 20px;
    overflow: hidden;
  }
</style>
