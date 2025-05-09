<script setup>
import { ref, reactive, onMounted } from 'vue'
import { gsap } from 'gsap'
import { Draggable } from 'gsap/Draggable'
import Toolbar from './components/toolbar.vue'

// Draggable gsap plugin

gsap.registerPlugin(Draggable)

onMounted(() => {
    Draggable.create("#yourID", {
        type: "x, y",
        dragClickables: true
    });

    Draggable.create("#draggable_card", {
        type: "x, y",
        dragClickables: true,
    });
})

//-------------------------------------------------------------------------------------------------------------------------------
// ANIMATION OKEY 
onMounted(() => {
  gsap.set('.card_wrapper', {
    x: window.innerWidth / 2 - 100,
    y: window.innerHeight / 2 - 100,
    opacity: 0
  })
  gsap.to('.card_wrapper', {
    x: 0,
    y: 0,
    opacity: 1,
    stagger: 0.2,
    duration: 1,
    ease: 'power2.out'
  })
})
//-------------------------------------------------------------------------------------------------------------------------------


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


//-------------------------------------------------------------------------------------------------------------------------------
// // SHUFFLE --> I am not understanding how this shuffles index and fucks up the game logic not touching the handle 
// const shuffleArray = (data_copy) => {
//   for (let i = card_array.length - 1; i > 0; i--) {
//     const j = Math.floor(Math.random() * (i + 1));
//     [card_array[i], card_array[j]] = [card_array[j], card_array[i]];
//   }
//   return data_copy;
// };

// shuffleArray()
//-------------------------------------------------------------------------------------------------------------------------------


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
        setTimeout(() => {
            parseCards(flipped_cards)
        }, "1000");

    }
    console.log("aaa");
    console.log(data_copy);
    console.log(card_array);


}
</script>


<template>
    <div class="app">
        <div class="cards_container">
            <div v-for="(card, index) in cards" :key="index" class="card_wrapper" id="draggable_card"
                @click="flipCard(index)">
                <div class="card cards_container_card" :class="{ flipped: card.flipped, locked: card.locked }">
                    <div class="card_face card_back">
                        <img src="https://placehold.co/100x150" alt="back" />
                    </div>
                    <div class="card_face card_front">
                        <img :src="card.interior" alt="interior" />
                    </div>
                </div>
            </div>
        </div>
        <OtherApps />
        <Toolbar :resetGame="resetGame" />
    </div>
</template>

<style lang="sass">
.app 
    height: 100vh
    background: #0867FF
    perspective: 1000px
    overflow: hidden
 
    .cards_container 
        margin: auto
        display: grid
        grid-template-columns: 1fr 1fr 1fr
        gap: 2rem
        max-width: 40vw
        .card_wrapper
            width: 100px
            height: 150px 
            text-align: center
            flex: 0 0 100px
            padding: 4px
            border: 1px solid Red
            position: relative
            cursor: pointer
            .cards_container_card 
                transform-style: preserve-3d 
                transition: transform 0.6s 
                &.flipped 
                    transform: rotateY(180deg)
                &.locked 
                    opacity: .5
                    pointer-events: none
            .card 
                width: 100%
                height: 100%
                position: relative
                transform-style: preserve-3d
                .card_face 
                    position: absolute
                    width: 100%
                    height: 100%
                    backface-visibility: hidden 
                    display: flex
                    align-items: center
                    justify-content: center                  
                .card_back 
                    background: white
                    img
                        width: 100%
                        height: 100%
                        object-fit: cover
                .card_front 
                    background: white
                    transform: rotateY(180deg) 
                    img
                        width: 100%
                        height: 100%
                        object-fit: cover
</style>