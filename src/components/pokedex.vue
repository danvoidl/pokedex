<template>
  <link
    href="https://fonts.googleapis.com/css2?family=DotGothic16&display=swap"
    rel="stylesheet"
  />
  <link
    href="https://fonts.googleapis.com/css2?family=Arvo&display=swap"
    rel="stylesheet"
  />

  <div class="gen">
    <button class="btn-gen" @click="getPokemon(1, 151)">1ª Gen</button>
    <button class="btn-gen" @click="getPokemon(152, 251)">2ª Gen</button>
    <button class="btn-gen" @click="getPokemon(252, 386)">3ª Gen</button>
    <button class="btn-gen" @click="getPokemon(387, 493)">4ª Gen</button>
    <button class="btn-gen" @click="getPokemon(495, 649)">5ª Gen</button>
    <button class="btn-gen" @click="getPokemon(650, 721)">6ª Gen</button>
    <button class="btn-gen" @click="getPokemon(722, 809)">7ª Gen</button>
    <button class="btn-gen" @click="getPokemon(810, 898)">8ª Gen</button>
  </div>

  <span class="searchpokemon">
    <input id="searchpoke" type="text" placeholder="Pokemon Name" />
    <button class="search" @click="getPokemonByName()">Pesquisar</button>
  </span>

  <div id="pokedex">
    <div :key="poke.id" v-for="poke in pokemon" class="card">
      <h2 class="pokeName">{{ poke.name }}</h2>

      <div v-if="poke.isLegendary" class="legendary">
        <p>Legendary</p>
      </div>

      <div v-if="poke.isMythical" class="mythical">
        <p>Mythical</p>
      </div>

      <img :src="poke.imgSrc" class="imgPoke" />

      <div class="stats">
        <span :key="stat" v-for="stat in poke.stats">
          <img
            v-if="stat.stat == 'hp'"
            style="width: 20px"
            src="../assets/heart.svg"
            alt=""
          />
          <img v-else src="../assets/swords.svg" style="width: 20px" alt="" />
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
    async getPokemon(begin, end) {
      //! 1ª Gen: 1 - 151
      //! 2ª Gen: 152 - 251
      //! 3ª Gen: 252 - 386
      //! 4ª Gen: 387 - 493
      //! 5ª Gen: 494 - 649
      //! 6ª Gen: 650 - 721
      //! 7ª Gen: 722 - 809
      //! 8ª Gen: 810 - 898

      this.pokemon = [];

      for (let index = begin; index <= end; index++) {
        let response = await fetch(
          `https://pokeapi.co/api/v2/pokemon/${index}/`
        );
        let data = await response.json();
        console.log(data);

        let response2 = await fetch(data.species.url);
        let data2 = await response2.json();

        console.log(data2);

        this.pokemon.push({
          name: data.name,
          imgSrc: data.sprites.front_default,
          types: data.types,
          isLegendary: data2.is_legendary,
          isMythical: data2.is_mythical,
          stats: [
            {
              stat: data.stats[0].stat.name,
              statValue: data.stats[0].base_stat,
            },
            {
              stat: data.stats[1].stat.name,
              statValue: data.stats[1].base_stat,
            },
          ],
        });
      }
    },
    async getPokemonByName() {
      this.pokemon = [];

      let name = document.getElementById("searchpoke").value;

      console.log(name);
      let response = await fetch(
        `https://pokeapi.co/api/v2/pokemon/${name.toLowerCase()}/`
      );
      let data = await response.json();

      console.log(data);

      let response2 = await fetch(data.species.url);
      let data2 = await response2.json();

      console.log(data2);

      this.pokemon.push({
        name: data.name,
        imgSrc: data.sprites.front_default,
        types: data.types,
        isLegendary: data2.is_legendary,
        isMythical: data2.is_mythical,
        stats: [
          {
            stat: data.stats[0].stat.name,
            statValue: data.stats[0].base_stat,
          },
          {
            stat: data.stats[1].stat.name,
            statValue: data.stats[1].base_stat,
          },
        ],
      });
    },
  },
  async created() {
    this.getPokemon(1, 3);
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
}

/* STYLES FOR THE GENERATION OPTIONS */
.gen {
  
}

@media (min-width: 300px) and (max-width: 450px) {
  .gen {
    flex-wrap: wrap;
    width: 90%;
  }

  .btn-gen {
    margin: 1%;
  }

  .card{
    margin-bottom: 4% !important;
    background-color:#5c95fc
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

#searchpoke {
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
  position: relative;
  display: grid;
  place-items: center;
}

.imgPoke {
  position: absolute;
}

.pokeName {
  width: 100%;
  background: black;
  color: white;
  text-transform: capitalize;
  text-align: center;
  position: absolute;
  top: 0;
  font-family: "Josefin Sans", sans-serif;
  font-size: 20px;
  padding: 2px;
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
}

/* STYLES FOR TYPES */
.bug {
  background: #afc421;
  box-shadow: 3px 3px 3px #afc421;
}

.dark {
  background: #6b5142;
}

.dragon {
  background: #7a3dff;
}

.electric {
  background: #f6d434;
  font-size: 14px;
}

.fighting {
  background: #c62922;
  font-size: 14px;
}

.fire {
  background: #e98049;
}

.flying {
  background: #967ed2;
}

.ghost {
  background: #644c8a;
}

.grass {
  background: #7cc359;
}

.ground {
  background: #debe5b;
}

.ice {
  background: #9adbdd;
}

.normal {
  background: #b6ba87;
}

.fairy {
  background: #ff87ab;
}

.poison {
  background: #a441ad;
  box-shadow: 1px 1px 3px #6c2b72;
}

.psychic {
  background: #ff417b;
}

.rock {
  background: #b59e2e;
}

.steel {
  background: #c1bfcd;
}

.water {
  background: #5c95fc;
}

.shadow {
  background: #7f6cbb;
}
</style>