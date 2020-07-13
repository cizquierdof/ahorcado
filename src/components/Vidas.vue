<template>
  <div class="vidas">
    <img v-bind:src="image[6 - vidas]" alt="" height="290px" />
    <div v-if="vidas">
      <h2>vidas Restantes: {{ vidas-1 }}</h2>
    </div>
    <div v-else>
      <h1>¡Has perdido!</h1>
      <button class=" btn btn-primary">Nueva partida</button>
    </div>
  </div>
</template>

<script>
import bus from "../bus";

export default {
  name: "Vidas",
  data() {
    return {
      vidas: 6,
      image: [
        "../assets/horca0.png",
        "../assets/horca1.png",
        "../assets/horca2.png",
        "../assets/horca3.png",
        "../assets/horca4.png",
        "../assets/horca5.png",
        "../assets/horca6.png",
      ],
      actualImage: "../assets/horca0.png",
    };
  },
  created() {
    this.actualImage = this.image[0];
    bus.$on("dec:vidas", (event) => {
      console.log("event", event);
      this.vidas = event;
    });
  },
  updated() {
    if (!this.vidas) {
      console.log("Has perdido!!");
      bus.$emit("fin:partida", true);
    }
  },
};
</script>

<style>
.vidas {
  text-align: center;
}
</style>
