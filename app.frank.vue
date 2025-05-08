<script setup>
import { ref, reactive, onMounted } from 'vue'
import { gsap } from 'gsap'
import { Draggable } from 'gsap/Draggable'
import Toolbar from './components/toolbar.vue'

// Draggable gsap plugin
gsap.registerPlugin(Draggable)

// DATA LAYER WITH ONLY THE DATA I ACTUALLY REQUIRE ( no doubles, no shuffle )
// THINK TOP DOWN 

const data = [{
    handle: "dog",
    interior: "https://picsum.photos/id/237/200/300",
    locked: false,
    flipped: false
},
{
    handle: "banana",
    interior: "https://picsum.photos/id/123/200/300",
    locked: false,
    flipped: false
},
{
    handle: "pear",
    interior: "https://picsum.photos/id/1/200/300",
    locked: false,
    flipped: false
}
];


// DIFF BETWEEN CLONE ( copies as reference ) AND DEEPCLONE ( creates new objects also if duplicated )
const data_copy = structuredClone(data);
// make an unique array 
const card_array = data.concat(data_copy);
// HERE WE SHOULD SHUFFLE THE ARRAY

const cards = ref(card_array)


// LOGIC HERE IS THAT I PASS THE ARRAY OF THE TWO FILPPED CARDS. I CHECK IF THEY ARE MATCHING. 
// THEN BY LOPPING THE ARRRAY I FOUND THE REF IN THE ORIGINAL ARRAY AND CHANGE THE DATA BASED ON THE MATCH
// AT THE END I LAUNCH THE CHECKRESUT FUNCTION TO HANDLE THE STATE OF THE GAME
const parseCards = function (flipped_cards) {
    const result = (flipped_cards[0].handle == flipped_cards[1].handle)
    flipped_cards.forEach((el) => {
        cards.value[el.index].locked = result ? true : false
        cards.value[el.index].flipped = result ? true : false
    })
    checkResults();
}
// I COMPARE THE LENGTH OF A FILTERED ARRAY BASED ON THE LOCKED FIELD. 
// IF THE LENGTH IS THE SAME AS THE CARD ARRAY IT'S A WIN

const checkResults = function () {
    const locked_cards = cards.value.filter((el) => el.locked)
    if (locked_cards.length == cards.value.length) {
        alert("BAZINGA")
        resetGame()
    }
}

// LOOP TROUGHT THE ORIGINAL ARRAY AND RESET THE STATUS
const resetGame = function () {
    cards.value.forEach((el) => {
        el.locked = false;
        el.flipped = false;
    })
}

// WE ARE WORKING WITH CLASS BASED ANIMATION - MAYBE GSAP TO BE SPECTACULAR OR EASIER TO HANDLE ( TIMING ETC.)
const flipCard = function (index) {
    cards.value[index].flipped = !cards.value[index].flipped
    // THIS NEXT LINE MAKES THE MAGIC- BY ASSIGNING THE INDEX OF THE CARD TO THE OBJECT IN THE ARRAY I HAVE AN UNIQ REF.
    cards.value[index].index = index
    const flipped_cards = cards.value.filter((el) => el.flipped && !el.locked)
    if (flipped_cards.length == 2) {
        parseCards(flipped_cards)
    }

}
</script>


<template>
    <div class="app">
        <div class="cards_container">
            <div v-for="(card, index) in cards"
                :class="['cards_container_card', { flipped: card.flipped }, { locked: card.locked }]"
                @click="flipCard(index)">
                <p>{{ card.handle }} {{ index }}</p>
            </div>
        </div>
        <OtherApps />
        <Toolbar />
    </div>
</template>



<style lang="scss">
* {
    border-sizing: border-box;
}

.cards_container {
    display: flex;
    gap: 4px;

    &_card {
        width: 100px;
        height: 100px;
        text-align: center;
        flex: 0 0 100px;
        padding: 4px;
        border: 1px solid Red;
        display: flex;
        align-items: center;
        justify-content: center;

        &.flipped {
            background-color: Red;
        }

        &.locked {
            opacity: .5;
            pointer-events: none;
        }
    }
}
</style>
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