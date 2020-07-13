<template>
  <div class="word">
    <div v-if="secretWord">
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
    <div v-else>...loading</div>
  </div>
</template>

<script>
import bus from "../bus";
export default {
  data() {
    return {
      acertadas: [],
      letra: "",
      listaLetras: [],
      vidas: 6,
    };
  },
  props: ["secretWord"],
  mounted() {},
  created() {
    for (let index = 0; index < this.secretWord.length; index++) {
      this.acertadas[index] = "_";
      //console.log('acertadas', this.acertadas);
    }
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
  },
};
</script>

<style>
.word {
  padding: 0 1.5em;
  font-size: 1.5em;
}
</style>
