<script context="module">

    export async function load({page}) {
        const url = `https://pokeapi.co/api/v2/pokemon?limit=150`;
        const res = await fetch(url);
        const data = await res.json();
        const loadedPokemon = data.results.map((data, index) => {
            return {
                name: data.name,
                id: index + 1,
                image: `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${index + 1}.png`
            }
        })
        return {
            props: {
                pokemons: loadedPokemon
            }
        };
    }
</script>


<script lang="ts">

    import Pokemoncard from "../components/pokemoncard.svelte";

    export let pokemons;
    let searchTerm = "";
    let filteredPokemon = [];

    $:  {
        console.log(searchTerm);
        if (searchTerm) {
            // search the Pokemon
            filteredPokemon = pokemons.filter(pokemon => 
                pokemon.name.toLowerCase().includes(searchTerm.toLowerCase())
            )
        }
        else {
            filteredPokemon = [...pokemons]
        }
    }    

</script>

<svelte:head>
    <title>Svelte Kit</title>
</svelte:head>


<h1 class="text-4xl text-center my-8 uppercase">Svelte Kit Pokedex</h1>
<input 
    class="w-full rounded-md text-lg p-4 border-2 border-gray-200" 
    type="text"
    bind:value={searchTerm}
    placeholder="Search Pokemon"
>

<div class="grid gap-4 py-4 md:grid-cols-2 grid-cols-1">
    {#each filteredPokemon as pokemon}
        <Pokemoncard pokemon={pokemon}/>
    {/each}
</div>


