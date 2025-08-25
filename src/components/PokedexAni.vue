<template>
  <div>
    <div class="conteiner">
      <div class="pokedex">
        <img v-if="imagem" :src="imagem" alt="" class="pokemom_image" />
        <img src="../assets/icons/pokedex.png" alt="" />

        <h1 v-if="numero && nome" class="pokemonData">
          <p class="pokemonNumber">{{ numero }}</p>
          <p style="padding-left: 3%; padding-right: 3%">-</p>
          <p class="pokemonName">{{ nome }}</p>
        </h1>
        <h1 v-if="erro" class="pokemonData">
          {{ erro }}
        </h1>
        <form class="form" @submit.prevent="fetchpokemon">
          <input
            type="search"
            class="inputSeach"
            placeholder="Name or Number"
            required
            spellcheck="false"
            :value="pokemon"
            @input="pokemon = $event.target.value"
          />
        </form>
        <div class="buttons">
          <button class="button btn-prev" @click="trasPoke">Prev &lt;</button>
          <button class="button btn-next" @click="proxPoke">Next &gt;</button>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import axios from "axios";
export default {
  data() {
    return {
      nome: "",
      numero: "",
      pokemon: "",
      imagem: "",
      erro: "",
    };
  },
  // created() {
  //   this.fetchpokemon();
  // },
  methods: {
    async proxPoke() {
      const id = Number(this.numero) + 1;
      await this.fetchpokemon(id);
    },
    async trasPoke() {
      const id = Number(this.numero) - 1;
      await this.fetchpokemon(id);
    },
    async fetchpokemon(id) {
      this.nome = "";
      this.numero = "";
      this.imagem = "";
      this.erro = "";
      const buscaPokemon = this.pokemon || id;
      try {
        this.pokemon = "Loading...";
        const APIResponse = await axios.get(
          `https://pokeapi.co/api/v2/pokemon/${String(
            buscaPokemon
          ).toLowerCase()}`
        );

        const data = APIResponse.data;
        console.log(data);
        this.nome = data.name;
        this.numero = data.id;
        this.imagem =
          data["sprites"]["versions"]["generation-v"]["black-white"][
            "animated"
          ]["front_default"];
        this.pokemon = "";
      } catch (error) {
        this.erro = "nao encontrado :(";
        this.pokemon = "";
      }
    },
  },
};
</script>
<style scoped>
.conteiner {
  text-align: center;
  font-family: "Oxanium", cursive;
  background: linear-gradient(to bottom, rgb(0, 183, 255), #fff);
  display: flex;
  justify-content: center;
  align-items: center;
}
.pokedex {
  width: 100%;
  max-width: 425px;
  padding-top: 0.3rem;
  position: relative;
}
.pokemom_image {
  position: absolute;
  left: 0;
  width: 25%;
  height: 15=7%;
  bottom: 53%;
  left: 33%;
}
.pokemonData {
  position: absolute;
  font-weight: 600;
  color: #aaa;
  bottom: 40.5%;
  right: 25%;
  display: flex;
  font-size: 130%;
}
.pokemonName {
  color: #3a444d;
  text-transform: capitalize;
}

.form {
  position: absolute;

  width: 65%;
  bottom: 25%;
  left: 11%;
}
.inputSeach {
  width: 108%;
  padding: 5%;
  outline: none;
  border: 2px solid #333;
  border-radius: 5px;
  font-weight: 600;
  color: #3a444d;
  font-size: 100%;
  -webkit-box-shadow: -1px 5px 0px 1px rgba(66, 63, 66, 1);
  -moz-box-shadow: -1px 5px 0px 1px rgba(66, 63, 66, 1);
  box-shadow: -1px 5px 0px 1px rgba(66, 63, 66, 1);
}
.buttons {
  position: absolute;
  bottom: 10%;
  left: 50%;
  width: 65%;

  transform: translate(-57%, 0);
  display: flex;
  gap: 20px;
}
.button {
  width: 50%;
  padding: 4%;
  border: 2px solid #000;
  border-radius: 5px;
  font-size: clamp(8px, 5vw, 1rem);
  font-weight: 600;
  color: white;
  background-color: #444;
  box-shadow: 2px 3px 0 #222, -2px 7px 0 #000;
}
.button:active {
  box-shadow: inset -4px 4px 0 #222;
  font-size: 0.9rem;
}
</style>
