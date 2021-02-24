<template>
  <link href="https://fonts.googleapis.com/css2?family=DotGothic16&display=swap" rel="stylesheet">
  <link href="https://fonts.googleapis.com/css2?family=Arvo&display=swap" rel="stylesheet">
  <link href="https://fonts.googleapis.com/css2?family=Carter+One&display=swap" rel="stylesheet">

  <div id="pokedex">
    <div :key="poke.id" v-for="poke in pokemon" class="card">

      <h2 v-if="poke.isMythical" class="pokeNameMythical" >{{ poke.name }} </h2>
         
      <h2 class="pokeName" v-else >{{ poke.name }} </h2>
      
      <div v-if="poke.isLegendary" class='legendary'>
        <p>Legendary</p>
      </div>

      <div v-if="poke.isMythical" class='mythical'>
        <p>Mythical</p>
      </div>
      <img src="../assets/pokeball.png" style="width: 55%; height: 40%;" class='pokeball' alt="">
      <img :src="poke.imgSrc" class="imgPoke">
      
      <div class="stats">
          <span :key='stat' v-for='stat in poke.stats'>
            <img v-if="stat.stat == 'hp'" style="width: 20px;" src="../assets/heart.svg" alt="">
            <img v-else src="../assets/swords.svg" style="width: 20px;" alt="">
            <p> {{ stat.statValue }} </p>
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
      pokemon: []
    };
  },
  //* Function to get pokemon
  async created(){
    for(let index = 440; index <= 600; index++) {
      let response = await fetch(`https://pokeapi.co/api/v2/pokemon/${index}/`);
      let data = await response.json();
      console.log(data);

      let response2 = await fetch(data.species.url);
      let data2 = await response2.json();  

      console.log(data2)

      this.pokemon.push({
        name: data.name,
        imgSrc: data.sprites.front_default,
        types: data.types,
        isLegendary: data2.is_legendary,
        isMythical: data2.is_mythical,
        stats: [
          {
            stat: data.stats[0].stat.name,
            statValue: data.stats[0].base_stat
          },
          {
            stat: data.stats[1].stat.name,
            statValue: data.stats[1].base_stat
          }
        ], 
      })
    }
  }
};
</script>

<style>
*{
  margin: 0;
  padding: 0;
}


#pokedex {
  width: 90%;
  margin: auto;
  display: flex;
  flex-wrap: wrap;
}

.card {
  width: 150px;
  height: 200px;
  margin: 2%;
  border: 1px solid black;
  position: relative;
  display: grid;
  place-items: center;
  transition: 1s;
  
}

.pokeNameMythical{
  background: black;
  width: 100%;
  position: absolute;
  top: 0;
  text-align: center;
  text-transform: capitalize;
  font-family: 'Carter One', cursive;
  color: white;
  transform: scale(1.01);
}

.imgPoke{
  position:absolute;
}

.pokeball{
  opacity: 0.7;
}

.pokeName {
  width: 100%;
  background: black;
  color: white;
  text-transform: capitalize;
  text-align: center;
  position: absolute;
  top: 0;
  font-family: 'Carter One', cursive;
  font-size: 20px;
  padding: 2px;
}


.legendary{
  position: absolute;
  left: -5%;
  top: 25%;
  height: 50%;
  display: flex;
  text-align: center;
  font-size: 10px;
}

.legendary p{
  border-radius: 5px;
  background-color: yellow;
  writing-mode: vertical-lr;
  text-orientation: upright;
  font-family: 'DotGothic16', sans-serif;
}

@keyframes mythicalAnimation {
  0% { 
    transform: scale(0.9);
  }
  100% {
    transform: scale(1)
  }

}

.mythical{
  position: absolute;
  left: -5%;
  top: 25%;
  height: 50%;
  display: flex;
  text-align: center;
  font-size: 10px;
  animation: mythicalAnimation 600ms ease-in infinite alternate;
}

.mythical p{
  border-radius: 5px;
  background-color: #1F0322;
  color: white;
  writing-mode: vertical-lr;
  text-orientation: upright;
  font-family: 'DotGothic16', sans-serif;
}
.stats{
  position: absolute;
  top: 75%;
  width: 100%;
  display: flex;
  justify-content: space-around;
  text-transform: capitalize;
  font-size: 15px;
}

.stats span{
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.stats span p{
  color: black;
}

.type{
  width: 100%;
  display: flex;
  justify-content:space-around;
  position: absolute;
  bottom: -5%;
}

.type p{
  width: 40%;
  text-align: center;
  padding: 2px;
  text-transform: capitalize;
  border-radius: 2px;
  font-family: 'DotGothic16', sans-serif;
}

/* STYLES FOR TYPES */

.bug{
    background: #afc421;
    box-shadow: 3px 3px 3px #afc421;
}

.dark{
    background: #6b5142;
}

.dragon{
    background: #7a3dff;
}

.electric{
    background: #f6d434;
    font-size: 14px;
}

.fighting{
    background: #c62922;
    font-size: 14px;
}

.fire{
    background:#e98049;
}

.flying{
    background: #967ed2
}

.ghost{
    background: #644c8a
}

.grass{
    background: #7cc359
}

.ground{
    background: #debe5b
}

.ice{
    background: #9adbdd;
}

.normal{
    background: #b6ba87;
}

.fairy{
  background: #ff87ab
}

.poison{
    background: #a441ad;
    box-shadow: 1px 1px 3px #6c2b72;
}

.psychic{
    background: #ff417b
}

.rock{
    background: #b59e2e
}

.steel{
    background: #c1bfcd
}

.water{
    background: #5c95fc
}

.shadow{
    background: #7f6cbb
}

</style>