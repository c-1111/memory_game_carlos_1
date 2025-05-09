<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'
const props = defineProps({
    resetGame: {
        type: Function,
        required: true
    },
    initGame: {
        type: Function,
        required: true
    }
});


const currentTime = ref(new Date())

let timer = null

onMounted(() => {
    timer = setInterval(() => {
        currentTime.value = new Date()
    }, 1000) // 30 secondi
})

onBeforeUnmount(() => {
    clearInterval(timer)
})
</script>

<template>
    <div class="toolbar_container">
        <div class="toolbar">
            <div class="toolbar_left">
                <div class="button" @click="initGame()">

                    <svg xmlns="http://www.w3.org/2000/svg" width="13" height="15" fill="none" viewBox="0 0 13 15">
                        <path fill="#000"
                            d="M6.5 15c-.903 0-1.748-.17-2.536-.509a6.608 6.608 0 0 1-2.059-1.375 6.529 6.529 0 0 1-1.39-2.035A6.172 6.172 0 0 1 0 8.57h1.444c0 1.393.491 2.575 1.472 3.545.982.97 2.176 1.456 3.584 1.455 1.408 0 2.603-.485 3.584-1.455.982-.97 1.473-2.15 1.472-3.545-.001-1.393-.492-2.575-1.472-3.544C9.104 4.057 7.91 3.572 6.5 3.57h-.108l1.12 1.108L6.5 5.714 3.611 2.857 6.5 0l1.011 1.036-1.12 1.107H6.5c.903 0 1.748.17 2.537.51a6.593 6.593 0 0 1 2.058 1.374 6.533 6.533 0 0 1 1.39 2.036c.344.78.516 1.616.515 2.508 0 .893-.172 1.73-.514 2.51a6.49 6.49 0 0 1-1.39 2.035 6.652 6.652 0 0 1-2.059 1.375c-.788.34-1.633.51-2.537.509Z" />
                    </svg>
                    <p>Restart</p>

                </div>
            </div>
            <div class="toolbar_right">
                <div class="volume_CTA">
                    <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="none" viewBox="0 0 16 16">
                        <path fill="#000"
                            d="M13.334 8a5.333 5.333 0 0 0-3.28-4.922L9.54 4.31A4 4 0 0 1 12 8a3.995 3.995 0 0 1-2.463 3.69l.513 1.232A5.331 5.331 0 0 0 13.334 8Zm-2.668 0c0-1.11-.678-2.06-1.641-2.46l-.513 1.232c.482.2.82.675.82 1.231 0 .553-.338 1.028-.82 1.231l.513 1.232A2.678 2.678 0 0 0 10.665 8Zm.412-7.384-.515 1.23a6.665 6.665 0 0 1 0 12.306l.512 1.232A7.999 7.999 0 0 0 11.078.615ZM0 4.666v6.665h2.666L7.334 16V0L2.666 4.666H0Z" />
                    </svg>
                </div>
                <div class="nightmode_CTA">
                    <svg xmlns="http://www.w3.org/2000/svg" width="14" height="17" fill="none" viewBox="0 0 14 17">
                        <path fill="#000"
                            d="M10.136 15.727a8.235 8.235 0 0 1-3.166.236 8.14 8.14 0 0 1-2.869-.89 7.562 7.562 0 0 1-2.268-1.858 6.903 6.903 0 0 1-1.365-2.662A6.927 6.927 0 0 1 .32 7.564a7.55 7.55 0 0 1 1.035-2.742 8.153 8.153 0 0 1 2.038-2.207 8.206 8.206 0 0 1 2.861-1.377A8.5 8.5 0 0 1 8.319.946a7.999 7.999 0 0 1 2.06.235c-1.022.972-1.73 2.107-2.121 3.402a6.974 6.974 0 0 0-.063 3.9 6.988 6.988 0 0 0 2.005 3.344c.986.927 2.166 1.555 3.538 1.886a8.03 8.03 0 0 1-1.668 1.233 8.526 8.526 0 0 1-1.934.78Z" />
                    </svg>
                </div>
                <div class="clock">
                    <p>{{ currentTime.toLocaleTimeString() }}</p>
                </div>
            </div>
        </div>
    </div>
</template>

<style lang="sass">
    .toolbar_container
        width: 100%
        position: fixed
        bottom: 0 
        font-family: Univers
        font-weight: bold
        .toolbar
            align-items: center
            padding-top: .4rem
            padding-bottom: 0.4rem
            padding-inline: 1rem
            background: #C9C9C9
            width: 100%
            height: 40px
            display: flex
            justify-content: space-between
            .toolbar_left
                .button
                    padding-inline: .6rem
                    background: #E6E6E6
                    display: flex
                    gap: .4rem
                    align-items: center
            .toolbar_right
                display: flex
                flex-direction: row
                gap: .4rem
                .volume_CTA
                    display: flex
                    align-items: center
                    background: #E6E6E6
                    padding-inline: .6rem
                .nightmode_CTA
                    display: flex
                    align-items: center
                    background: #E6E6E6
                    padding-inline: .6rem
                .clock
                    display: flex
                    align-items: center
                    background: #E6E6E6
                    padding-inline: .6rem
                    p

</style>