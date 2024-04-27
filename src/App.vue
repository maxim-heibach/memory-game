<template>
  <h1>Memory-Prototype</h1>
    <div id="app">
      Clicks: {{ clicks }}
      <br><br>
      <div class="grid-container">
        <div v-for="(card, index) in pictureCards" :key="index" @click="clickMethod(index)">
          <img v-if="card.discovered || card.showing" :src="card.pic" class="grid-item">
          <img v-else :src="background" class="grid-item">
        </div>
      </div>
    </div>
</template>

<script>
  export default {
    data() {
      return {
        background: 'img/question.svg',
        clicks: 0,
        pictureCards: [],
        discoveredCards: 0,
        selectedCards: []
      };
    },
    methods: {
      clickMethod(index) {
        if (this.selectedCards.length < 2 && !this.pictureCards[index].showing) {
          this.pictureCards[index].showing = true;
          this.selectedCards.push(index);
          this.clicks++;
          if (this.selectedCards.length === 2) {
            setTimeout(() => {
              this.checkMatch();
            }, 500);
          }
        }
      },
      checkMatch() {
        const [index1, index2] = this.selectedCards;
        if (this.pictureCards[index1].pic === this.pictureCards[index2].pic) {
          this.pictureCards[index1].discovered = true;
          this.pictureCards[index2].discovered = true;
          this.discoveredCards += 2;
          if (this.discoveredCards === this.pictureCards.length) {
            alert('Herzlichen Glückwunsch! Du hast alle Paare entdeckt.');
          }
        } else {
          this.pictureCards[index1].showing = false;
          this.pictureCards[index2].showing = false;
        }
        this.selectedCards = [];
      },
    },
    mounted() {
      var images = [
        {pic: 'img/javascript.png', discovered: false, showing: false},
        {pic: 'img/laravel.svg', discovered: false, showing: false},
        {pic: 'img/nodejs.png', discovered: false, showing: false},
        {pic: 'img/php.png', discovered: false, showing: false},
        {pic: 'img/typescript.png', discovered: false, showing: false},
        {pic: 'img/react.svg', discovered: false, showing: false},
        {pic: 'img/svelte.svg', discovered: false, showing: false},
        {pic: 'img/vue.svg', discovered: false, showing: false},
      ];
      const doubledImages = [...images, ...images];
      doubledImages.sort(() => Math.random() - 0.5);
      this.pictureCards = JSON.parse(JSON.stringify(doubledImages));
    },
  };
</script>

<style scoped>
  .grid-container {
    display: grid;
    grid-template-columns: repeat(4, 1fr); /* Vier Spalten */
    grid-column-gap: 10px;
    grid-row-gap: 50px;
    max-width: 660px;
    padding: 0;
    justify-content: center;
  }

  .grid-item {
    object-fit: contain;
    object-position: center;
    width: 100px;
    height: auto;   
  }
</style>