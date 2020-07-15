<template>
  <div class="word">
    <div v-if="!secretWord">...loading</div>
    <div v-else>
      <div><h2>Aciertos</h2></div>
      <span v-for="(letra, i) in acertadas" :key="i">
        {{ letra }}
      </span>
      <div>
        Tus respuestas:
        <div>
          <span v-for="letra in listaLetras" :key="letra">{{ letra }},</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import bus from "../bus";
import axios from "axios";

export default {
  data() {
    return {
      acertadas: [],
      letra: "",
      listaLetras: [],
      vidas: 6,
      secretWord: "",
      loading: true,
    };
  },
  mounted() {
    bus.$on("set:word", (e) => {
      this.secretWord = e;
      for (let index = 0; index < this.secretWord.length; index++) {
        this.acertadas[index] = "_";
      }
    });

    bus.$on("add:letra", (event) => {
      this.listaLetras.push(event);
      this.isInSolution(event);
    });
  },
  methods: {
    isInSolution(letra) {
      let fallo = true;
      this.secretWord.split("").map((l, index) => {
        if (l === letra) {
          this.acertadas[index] = l;
          fallo = false;
        }
      });
      console.log("fallo", fallo);
      fallo && this.vidas--;
      bus.$emit("dec:vidas", this.vidas);
    },
    setWord() {
      return axios.get("https://api.datamuse.com/words?rel_jja=yellow");
    },
  },
};
</script>

<style>
.word {
  padding: 0 1.5em;
  font-size: 1.5em;
}
</style>
