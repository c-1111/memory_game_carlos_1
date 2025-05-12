<script setup>
import { ref, onMounted } from 'vue'
import { gsap } from 'gsap'
import { Draggable } from 'gsap/Draggable'
import Cards from './components/cards.vue'

gsap.registerPlugin(Draggable)

// Shuffle function, pure and returning the shuffled array
function shuffleArray(array) {
    const newArray = [...array]
    for (let i = newArray.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1))
            ;[newArray[i], newArray[j]] = [newArray[j], newArray[i]]
    }
    return newArray
}

// Fetch data and shuffle server-side
const { data: cards } = await useAsyncData('cards', () => {
    const baseData = [
        { handle: "aaa", interior: "/img/1.gif", locked: false, flipped: false },
        { handle: "bbb", interior: "/img/(2).gif", locked: false, flipped: false },
        { handle: "ccc", interior: "/img/(3).gif", locked: false, flipped: false },
        { handle: "ddd", interior: "/img/(4).gif", locked: false, flipped: false },
        { handle: "eee", interior: "/img/(5).gif", locked: false, flipped: false },
        { handle: "fff", interior: "/img/(6).gif", locked: false, flipped: false },
        { handle: "ggg", interior: "/img/(7).gif", locked: false, flipped: false },
        { handle: "hhh", interior: "/img/(8).gif", locked: false, flipped: false },
        { handle: "iii", interior: "/img/(9).gif", locked: false, flipped: false },
    ]
    const duplicated = baseData.concat(structuredClone(baseData))
    return shuffleArray(duplicated)
})

//UTILITY CLASS

const isAnimating = ref(false);
// DOM refs

const startingPoint = ref(null)

// Animation setup
onMounted(() => {
    Draggable.create("#startingPoint", { type: "x,y", dragClickables: true })
    Draggable.create("#yourID", { type: "x,y", dragClickables: true })
    Draggable.create("#draggable_card", { type: "x,y", dragClickables: true })
    initGame()
})

function initGame() {
    if (isAnimating.value) {
        return false;
    }
    isAnimating.value = true;
    resetGame()

    const position = startingPoint.value.getBoundingClientRect()
    const x_center = window.innerWidth / 2
    const y_center = window.innerHeight / 2
    const x_origin = position.x - x_center
    const y_origin = position.y

    gsap.set('.card_wrapper', { x: x_origin, y: y_origin, scale: 0.5, opacity: 0 })
    gsap.to('.card_wrapper', {
        x: 0, y: 0, opacity: 1, stagger: 0.2, scale: 1, duration: 1, ease: 'power2.out', onComplete: () => {
            isAnimating.value = false;
        }
    })
}

// Logic functions
function resetGame() {
    cards.value.forEach(el => {
        el.locked = false
        el.flipped = false
    })
}

function parseCards(flipped_cards) {
    const result = flipped_cards[0].handle === flipped_cards[1].handle
    flipped_cards.forEach(el => {
        cards.value[el.index].locked = result
        cards.value[el.index].flipped = result
    })
    checkResults()
}

function checkResults() {
    const locked_cards = cards.value.filter(el => el.locked)
    if (locked_cards.length === cards.value.length) {
        alert("BAZINGA")
        resetGame()
    }
}

function flipCard(index) {
    cards.value[index].flipped = !cards.value[index].flipped
    cards.value[index].index = index
    const flipped_cards = cards.value.filter(el => el.flipped && !el.locked)
    if (flipped_cards.length === 2) {
        setTimeout(() => parseCards(flipped_cards), 1000)
    }
}
</script>

<template>
    <div class="app">
        <div class="starting_point" ref="startingPoint" id="startingPoint" @click="initGame">
            <img src="/public/img/zip.png" alt="">
            <p>MEMORY</p>
        </div>
        
        <Cards :cards="cards" @flip="flipCard" />
        <OtherApps />
        <Toolbar :resetGame="resetGame" :initGame="initGame" />
    </div>
</template>

<style lang="scss">
.starting_point {
    position: fixed;
    bottom: 200px;
    right: 200px;
    display: flex;
    flex-direction: column;
    align-items: center;

    img {
        height: 50px;
        margin-bottom: .3rem
    }

    p {
        font-family: Calibri;
        font-weight: lighter;
        color: white;
        text-shadow: 2px 2px 4px #000000;
    }
}
</style>
<style lang="sass">
.app 
    height: 100vh
    background: #0867FF
    perspective: 1000px
    overflow: hidden
    display: flex
    align-items: center
    justify-content: center
    padding-bottom: 10rem
    
</style>