<template>
  <div class="char-input">
    <div class="casillas">
      <div>
        <input
          type="text"
          id="entrada"
          size="1"
          maxlength="1"
          v-model="letra"
          v-on:keyup.esc="limpia()"
          v-on:keyup.enter="setLetra()"
          v-bind:disabled='finPartida'
        />
        <button 
          class="btn btn-primary"
           @click="setLetra()"
           :disabled='finPartida'
           >
           Vale!
           </button>
      </div>
    </div>
  </div>
</template>

<script>
import bus from "../bus";
export default {
  name: "CharInput",
  data() {
    return {
      letra: "",
      finPartida: false,
    };
  },
  mounted() {
    document.getElementById("entrada").focus();
    bus.$on('fin:partida', (event)=>{
      this.finPartida=event;
    })
  },
  methods: {
    setLetra() {
      const input = document.getElementById("entrada");
      this.letra = input.value;
      bus.$emit("add:letra", this.letra);
      this.letra = "";
      this.limpia();
      input.focus();
    },
    limpia() {
      document.getElementById("entrada").value = "";
    },
    reset() {
      this.continuar = true;
    },
  },
};
</script>

<style>
.casillas {
  font-size: 1.5em;
}

.casillas input {
  text-align: center;
}
</style>
