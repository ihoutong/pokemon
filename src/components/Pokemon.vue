<template>
<div id="pokemonList">
    <h1>Pokemons weak against {{type}}</h1>
    <ul>
        <li v-for="name in names" :key="name">{{name}}</li>
    </ul>
</div>
</template>

<script>
export default {
    name: 'pokemon',
    props: {
        names: Array,
        type: String
    },
}
</script>

<style lang="scss">
#pokemonList {
    height: 600px;
    overflow: auto;

    ul {
        list-style: none;
        columns: 3;
        
        li {
            margin: 10px 0;
        }
    }
}
</style>