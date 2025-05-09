<script setup>
import { ref, onMounted } from 'vue'
import { gsap } from 'gsap'
import { Draggable } from 'gsap/Draggable'

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
        { handle: "aaa", interior: "https://picsum.photos/id/237/200/300", locked: false, flipped: false },
        { handle: "bbb", interior: "https://picsum.photos/id/10/200/300", locked: false, flipped: false },
        { handle: "ccc", interior: "https://picsum.photos/id/1/200/300", locked: false, flipped: false },
        { handle: "ddd", interior: "https://picsum.photos/id/2/200/300", locked: false, flipped: false },
        { handle: "eee", interior: "https://picsum.photos/id/3/200/300", locked: false, flipped: false },
        { handle: "fff", interior: "https://picsum.photos/id/4/200/300", locked: false, flipped: false },
        { handle: "ggg", interior: "https://picsum.photos/id/5/200/300", locked: false, flipped: false },
        { handle: "hhh", interior: "https://picsum.photos/id/6/200/300", locked: false, flipped: false },
        { handle: "iii", interior: "https://picsum.photos/id/7/200/300", locked: false, flipped: false },
    ]
    const duplicated = baseData.concat(structuredClone(baseData))
    return shuffleArray(duplicated)
})

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
    resetGame()
    const position = startingPoint.value.getBoundingClientRect()
    const x_center = window.innerWidth / 2
    const y_center = window.innerHeight / 2
    const x_origin = position.x - x_center
    const y_origin = position.y

    gsap.set('.card_wrapper', { x: x_origin, y: y_origin, scale: 0.5, opacity: 0 })
    gsap.to('.card_wrapper', { x: 0, y: 0, opacity: 1, stagger: 0.2, scale: 1, duration: 1, ease: 'power2.out' })
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

        <div class="cards_container">
            <div v-for="(card, index) in cards" :key="index" class="card_wrapper" id="draggable_card"
                @click="flipCard(index)">
                <div class="card cards_container_card" :class="{ flipped: card.flipped, locked: card.locked }">
                    <div class="card_face card_back">
                        <img src="/public/img/card.png" alt="back" />
                    </div>
                    <div class="card_face card_front">
                        <img :src="card.interior" alt="interior" />
                    </div>
                </div>
            </div>
        </div>
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
    .cards_container 
        margin: auto
        display: grid
        grid-template-columns: 1fr 1fr 1fr 1fr 1fr 1fr 
        gap: 2rem
        .card_wrapper
            width: 100px
            height: 150px 
            text-align: center
            flex: 0 0 100px
            padding: 4px
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