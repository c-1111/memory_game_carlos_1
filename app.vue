<script setup>
import { ref, reactive, onMounted } from 'vue'
import { gsap } from 'gsap'
import { Draggable } from 'gsap/Draggable'
import Toolbar from './components/toolbar.vue'

// Draggable gsap plugin
gsap.registerPlugin(Draggable)

const selected = ref([]) // EMPTY ARRAY TO STORE THE CARDS CHOSEN (MAX 2)

//ARRAY WITH THE CARDS, I GUESS THERE HAS TO BE A WAY TO PRINT TO TIMES THE SAME ARRAY RATHER THAN HAVING THE CARDS DUPLICATE
const cards = reactive([{
  id: "1",
  interior: "https://placehold.co/100x150",
  locked: false
},
{
  id: "1",
  interior: "https://placehold.co/100x150",
  locked: false
},
{
  id: "2",
  interior: "https://placehold.co/100x150",
  locked: false
},
{
  id: "2",
  interior: "https://placehold.co/100x150",
  locked: false
},
{
  id: "3",
  interior: "https://placehold.co/100x150",
  locked: false
},
{
  id: "3",
  interior: "https://placehold.co/100x150",
  locked: false
},
]);


// const cardsDeal = 

const flipCard = (index) => {
  // CHANGE flipped STATE --> onClick
  cards[index].flipped = !cards[index].flipped



  const card = cards[index]

  // LOCK REST OF THE CARDS
  if (card.locked || selected.value.length >= 2) return
  card.locked = true
  selected.value.push({
    id: card.id,
    index
  }) // STORE IN selected (ARRAY) THE VALUES OF THE CARDS CHOSEN

  if (selected.value.length === 2) {
    const [firstCard, secondCard] = selected.value // COMPARE SELECTED CARDS VALUES 

    if (firstCard.id !== secondCard.id) {
      // TURN CARDS IF CARD CONTENTS ARE NOT EQUAL
      setTimeout(() => {
        cards[firstCard.index].locked = false
        cards[secondCard.index].locked = false

        cards[firstCard.index].flipped = false
        cards[secondCard.index].flipped = false
        selected.value = []
      }, 1000)
      console.log("not equal");

    } else {
      // YES, THOSE ARE EQUAL +
      selected.value = []
      console.log("Right");

    }
  }
}




onMounted(() => {
  Draggable.create("#yourID", {
    type: "x, y",
  });

  Draggable.create("#card_container", {
    type: "x, y",
  });
})
</script>


<template>
  <div class="app">
    <div class="aaa" id="yourID" style="width:30px;height:30px;background:red;"></div>

    <div class="cards_gallery">
      <div v-for="(card, index) in cards" :key="index" id="card_container" class="card_container" @click="flipCard(index)">
        <div id="card" :class="['card', { flipped: card.flipped }]">
          <div class="card_rear">
            <img src="https://placehold.co/100x150" alt="">
          </div>
          <div class="card_interior">
            <img :src="card.interior" alt="interior">
          </div>
        </div>
        <p>{{ card.id }} {{ card.locked }}</p>
      </div>
    </div>

    <OtherApps />
    <Toolbar />
  </div>
</template>




<style lang="sass">
.app
  background: #0867FF
  height: 100vh
  .cards_gallery
    display: flex
    flex-direction: row
    .card_container 
      border: solid 4px tomato
      display: flex
      flex-direction: row
      gap: 1rem
      perspective: 800px 
      .card 
        width: 50px 
        height: 100px  
        position: relative
        transform-style: preserve-3d 
        transition: transform 0.6s 
        .card_rear,
        .card_interior 
          position: absolute
          top: 0
          left: 0
          right: 0
          bottom: 0
          backface-visibility: hidden  
        .card_interior 
          transform: rotateY(180deg) 
      .card.flipped 
        transform: rotateY(180deg) 
</style>