<script>
    import { onMount } from 'svelte';
    import { page } from '$app/stores';
  
    let name;
    let type;
    let dimension;
    let residents = [];
    let residentCount = 0;
  
    onMount(async () => {
      const location_id = $page.url.searchParams.get('id');
      
  
      try {
        const response = await fetch(`https://rickandmortyapi.com/api/location/${location_id}`);
        const data = await response.json();
  
        name = data.name;
        type = data.type;
        dimension = data.dimension;
        residents = data.residents;
        console.log(residents)
      } catch (error) {
        console.error('Error:', error);
      }
    });
  
    $: {
      residentCount = residents.length;
    }
  </script>
  
  <div>
    <h1>{name}</h1>
    <h3>{type} | {dimension}</h3>
    <h5>{residentCount} residents</h5>
  </div>
  