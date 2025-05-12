<script setup>
defineProps({
    cards: Array
})

const emit = defineEmits(['flip'])

function flipCard(index) {
    emit('flip', index)
}</script>
<template>
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
</template>

<style lang="sass" scoped>
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
            transition: transform ease 0.6s 
            &.flipped 
                transform: rotateY(180deg) scale(1.2)
            &.locked 
                opacity: 0
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
                //background: white
                transform: rotateY(180deg) 
                img
                    //width: 100%
                    //height: 100%
                    width: 100%
                    height: auto
                    display: block
                    object-fit: contain</style>