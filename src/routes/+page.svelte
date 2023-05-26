<style>

:root {
  --main-color: white;
  --main-font: sans-serif;
  --main-font-size: 16px;
}

.grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 30px;
}

.card {
  background: #333;
  overflow: hidden;
  height: 240px;
  display: flex;
  border-radius: 20px;
}

.image {
  width: 30%;
  background: center / cover;
}

.info {
  padding: 10px 20px 15px;
}

.dead, .alive , .unknown {
  background: green;
  width: 10px;
  height: 10px;
  border-radius: 100%;
  margin-right: 10px;
}


.dead {
  background: firebrick;
}
.unknown {
  background: blue;
}

.name, .status, .location-label, .location, .location a {
  font-family: var(--main-font);
  color: var(--main-color);
}

.status {
  display: flex;
  align-items: center;
}

.location-label {
  color: #777;
  font-size: var(--main-font-size);
  margin-bottom: 5px;
}

</style>


<script>
import { onMount } from 'svelte';
let characterData = [];

onMount(async () => {
  const url = 'https://rickandmortyapi.com/api/character';

  try {
    const response = await fetch(url);
    if (response.ok) {
      const data = await response.json();
      const allCharacters = data.results;

      const randomCharacters = getRandomCharacters(allCharacters, 6);
      characterData = randomCharacters;
        
      console.log('Random characters:', characterData);
    } else {
      console.error('Failed to fetch data from the API.');
    }
  } catch (error) {
    console.error('An error occurred while fetching data:', error);
  }
});

function getRandomCharacters(characters, count) {
  const shuffled = characters.sort(() => 0.5 - Math.random());
  return shuffled.slice(0, count);
}

function handleLocationClick(event, id) {
 console.log(id);
    event.preventDefault(); // Prevent the default link navigation

  // Replace '/location' with the actual path to your LocationPage component
  window.location.href = '/location?id=' + id;
}

</script>

<div class="grid">
    {#each characterData as character}
      <div class="card" on:click={() => handleCharacterClick(character.id)}>
        <div class="image" style="background-image: url({character.image})"></div>
        <div class="info">
          <h1 class="name">{character.name}</h1>
          <h3 class="status">
            <div class={character.status === 'Dead' ? 'dead' : character.status === 'Unknown' ? 'unknown' : 'alive'}></div>

            {character.status} - {character.species}
          </h3>
          <h3 class="locationLabel">Last known location</h3>
          <h3 class="location">
            <a href="src\routes\location\+page.svelte" on:click={(event) => handleLocationClick(event, character.id)}>{character.location.name}</a>
          </h3>
        </div>
      </div>
    {/each}
  </div>