<template>
  <link
    href="https://fonts.googleapis.com/css2?family=DotGothic16&display=swap"
    rel="stylesheet"
  />
  <link
    href="https://fonts.googleapis.com/css2?family=Arvo&display=swap"
    rel="stylesheet"
  />

  <link
    href="https://fonts.googleapis.com/css2?family=VT323&display=swap"
    rel="stylesheet"
  />

  <div class="gen">
    <button class="btn-gen" @click="getGeneration(1, 151)">1ª Gen</button>
    <button class="btn-gen" @click="getGeneration(152, 251)">2ª Gen</button>
    <button class="btn-gen" @click="getGeneration(252, 386)">3ª Gen</button>
    <button class="btn-gen" @click="getGeneration(387, 493)">4ª Gen</button>
    <button class="btn-gen" @click="getGeneration(495, 649)">5ª Gen</button>
    <button class="btn-gen" @click="getGeneration(650, 721)">6ª Gen</button>
    <button class="btn-gen" @click="getGeneration(722, 809)">7ª Gen</button>
    <button class="btn-gen" @click="getGeneration(810, 898)">8ª Gen</button>
  </div>

  <span class="searchpokemon">
    <input class="searchpoke" type="text" placeholder="Pokemon Name" />
    <button class="search" @click="getPokemonByName()">Pesquisar</button>
  </span>

  <div id="pokedex">
    <div :key="poke.id" v-for="poke in pokemon" class="card">
      <div class="pokeName">
        <h2 :title="poke.name">
          {{
            poke.name.length > 10
              ? `${poke.name.substring(0, 8)}...`
              : poke.name
          }}
        </h2>
      </div>

      <p class="pokeId">#{{ poke.id }}</p>

      <div v-if="poke.isLegendary" class="legendary">
        <p>Legendary</p>
      </div>

      <div v-if="poke.isMythical" class="mythical">
        <p>Mythical</p>
      </div>

      <img
        src="../assets/pokeball.svg"
        style="width: 60%; height: 50%; opacity: 60%"
      />
      <img :src="poke.imgSrc" v-if="poke.isMythical" class="imgMythical" />
      <img :src="poke.imgSrc" class="imgPoke" :alt="poke.name" v-else />

      <div class="stats">
        <span :key="stat" v-for="stat in poke.stats">
          <img
            v-if="stat.stat == 'hp'"
            style="width: 20px"
            src="../assets/heart.svg"
          />
          <img v-else src="../assets/swords.svg" style="width: 20px" />
          <p>{{ stat.statValue }}</p>
        </span>
      </div>

      <div class="type">
        <p :key="el.slot" v-for="el in poke.types" :class="el.type.name">
          {{ el.type.name }}
        </p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      pokemon: [],
    };
  },
  //* Function to get pokemon
  methods: {
    async getGeneration(begin, end) {
      //! | 1ª Gen  | 2ª Gen    | 3ª Gen    | 4ª Gen    | 5ª Gen    | 6ª Gen    | 7ª Gen    | 8ª Gen
      //! | 1 - 151 | 152 - 251 | 252 - 386 | 387 - 493 | 494 - 649 | 650 - 721 | 722 - 809 | 810 - 898

      this.pokemon = [];

      for (let index = begin; index <= end; index++) {
        await this.getPokemon(index);
      }
    },
    async getPokemonByName() {
      this.pokemon = [];
      let name = document.querySelector(".searchpoke").value;

      await this.getPokemon(name.toLowerCase());
    },
    feedArray(pokemon, pokemonStatus) {
      this.pokemon.push({
        id: pokemon.id,
        name: pokemon.name,
        imgSrc: pokemon.sprites.front_default,
        types: pokemon.types,
        isLegendary: pokemonStatus.is_legendary,
        isMythical: pokemonStatus.is_mythical,
        stats: [
          {
            stat: pokemon.stats[0].stat.name,
            statValue: pokemon.stats[0].base_stat,
          },
          {
            stat: pokemon.stats[1].stat.name,
            statValue: pokemon.stats[1].base_stat,
          },
        ],
      });
    },
    async getPokemon(index) {
      let data = await fetch(`https://pokeapi.co/api/v2/pokemon/${index}/`);
      if (data.status === 404) {
        console.error(
          `Não foi possível obter o pokemon ${index}, erro ${data.status}`
        );
      }
      var pokemonData = await data.json();

      console.log(pokemonData);
      
      let status = await fetch(pokemonData.species.url);
      let pokemonStatus = await status.json();

      this.feedArray(pokemonData, pokemonStatus);
    },
  },
  async created() {
    await this.getGeneration(150, 151);
  },
};
</script>

<style>
@import url("stylesForTypes.css");

* {
  margin: 0;
  padding: 0;
}

/* STYLES FOR THE GENERATION OPTIONS */
.gen {
  margin: auto;
  margin-top: 2%;
  width: 90%;
  display: flex;
  justify-content: space-around;
}

@media (min-width: 300px) and (max-width: 450px) {
  .gen {
    flex-wrap: nowrap;
    width: 90%;
  }

  .btn-gen {
    margin: 1%;
  }

  .card {
    margin: 2% !important;
    margin-bottom: 4% !important;
  }

  #pokedex {
    width: 95% !important;
    margin-top: 5% !important;
  }
}

.btn-gen {
  font-family: "Josefin Sans", sans-serif;
  background: none;
  border: 1px solid black;
}

/* STYLES FOR THE SEARCH INPUT AND BUTTON */
.searchpokemon {
  width: 100%;
  margin-top: 2%;
  display: flex;
  justify-content: center;
}

.searchpoke {
  background: none;
  border: 1px solid black;
  padding: 5px;
  font-family: "Josefin Sans", sans-serif;
}

.search {
  margin-left: 0.5%;
  font-family: "Josefin Sans", sans-serif;
  background: none;
  border: 1px solid black;
  padding: 2px;
}

/* STYLES FOR THE POKEDEX */
#pokedex {
  width: 90%;
  margin: auto;
  display: flex;
  flex-wrap: wrap;
}

/* STYLES FOR THE CARD PROPERTIES */
.card {
  width: 150px;
  height: 200px;
  margin: 2%;
  border: 1px solid black;
  border-top: none;
  position: relative;
  display: grid;
  place-items: center;
  backdrop-filter: blur(5px);
  transition: 200ms;
}

.card:hover {
  transform: scale(1.05);
}

.imgPoke {
  position: absolute;
}

a {
  text-decoration: none;
}

.pokeName {
  width: 100%;
  background: black;
  position: absolute;
  top: 0;
}

.pokeName h2 {
  color: white;
  padding: 3px;
  text-transform: capitalize;
  text-align: center;
  font-family: "Josefin Sans", sans-serif;
  font-size: 18px;
  cursor: initial;
}

.pokeId {
  font-family: "Josefin Sans", sans-serif;
  top: 15%;
  position: absolute;
}

/* STYLES FOR STATUS */
.stats {
  position: absolute;
  top: 75%;
  width: 100%;
  display: flex;
  justify-content: space-around;
  text-transform: capitalize;
  font-size: 15px;
}

.stats span {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.stats span p {
  color: black;
  font-family: "DotGothic16", sans-serif;
}

/* STYLES FOR TYPE */
.type {
  width: 100%;
  display: flex;
  justify-content: space-around;
  position: absolute;
  bottom: -5%;
}

.type p {
  width: 40%;
  text-align: center;
  padding: 2px;
  text-transform: capitalize;
  border-radius: 2px;
  font-family: "DotGothic16", sans-serif;
}

/* STYLES FOR THE RARITY */
.legendary {
  position: absolute;
  left: -5%;
  top: 25%;
  height: 50%;
  display: flex;
  text-align: center;
  font-size: 10px;
}

.legendary p {
  border-radius: 5px;
  background-color: yellow;
  writing-mode: vertical-lr;
  text-orientation: upright;
  font-family: "DotGothic16", sans-serif;
}

.mythical {
  position: absolute;
  left: -5%;
  top: 25%;
  height: 50%;
  display: flex;
  text-align: center;
  font-size: 10px;
}

.mythical p {
  border-radius: 5px;
  background-color: #1f0322;
  color: white;
  writing-mode: vertical-lr;
  text-orientation: upright;
  font-family: "DotGothic16", sans-serif;
  animation: 1s linear 1s infinite alternate mityhcalanimation;
}

.imgMythical {
  position: absolute;
  animation: 1s linear 1s infinite alternate mityhcalanimation;
}

@keyframes mityhcalanimation {
  from {
    transform: scale(1);
    color: ghostwhite;
  }
  to {
    transform: scale(1.1);
    color: gold;
  }
}

/* STYLES FOR TYPES */
</style>