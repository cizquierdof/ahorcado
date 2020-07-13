<template>
  <div class="game">
    <div class="side">
      <div class=".title">
        <h3>Juego del ahorcado</h3>
      </div>
      <word  :secretWord='secretWord' />
      <charInput />
    </div>
    <vidas />
  </div>
</template>

<script>
import vidas from "./Vidas";
import charInput from "./CharInput";
import word from "./Word";
import axios from 'axios';


export default {
  name: "Game",
  components: {
    vidas,
    word,
    charInput,
  },
  data() {
    return {
      secretWord: '',
      loading: true
    }
  },
  mounted() {
    this.secretWord='patata'
  },
  updated() {
    
  },
  methods: {
    setWord(){
    axios.get('https://api.datamuse.com/words?rel_jja=yellow') 
    .then(res=> {
      console.log('data',res.data[0].word)
  //bus.$emit('set:word', res.data.records[0])
      //this.setWord(res.data.records[0])
      })
      .finally(()=>this.loading=false)
    }
  }
};
</script>

<style>
.game {
  width: 620px;
  margin: 1em auto 0 auto;
  padding: 20px;
  display: grid;
  grid-template-columns: 350px 250px;
  grid-template-rows: 400px;
  gap: 10px 10px;
  
}

.side {
  display: grid;
  grid-template-columns: 1fr;
  grid-template-rows: 2em 15em 5em 1fr;
  gap: 10px 1px;
  grid-area: 1 / 2 / 2 / 3;
  background-color:  #eeece7;
}

.title {
  grid-area: 1 / 1 / 2 / 2;
  border: solid red;
}

.word {
  grid-area: 2 / 1 / 3 / 2;
}

.casillas div {
  float: right;
  margin: 5px auto 0 auto;
  background-color:  white;
}


.char-input {
  grid-area: 3 / 1 / 4 / 2;
  
}

.vidas {
  grid-area: 1 / 1 / 2 / 2;
  background-color:  #eeece7;
}
</style>
