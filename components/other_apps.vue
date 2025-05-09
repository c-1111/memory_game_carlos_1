<script setup>
import { gsap } from 'gsap'
import { Draggable } from 'gsap/Draggable'

// Draggable gsap plugin
gsap.registerPlugin(Draggable)

onMounted(() => {
  Draggable.create("#app1", {
    type: "x, y",
  });
  Draggable.create("#app2", {
    type: "x, y",
  });
  Draggable.create("#deck", {
    type: 'x,y',
    dragClickables: true,
  })
})
</script>

<template>
    <div class="app1" id="app1"></div>
    <div class="app2" id="app2"></div>
    <div id="Memory" class="deck" @click="dealCards">Memory</div>
</template>

<style lang="sass">
.app1
    position: fixed
    right: 500px
    width: 100px
    height: 100px
    background: tomato

.app2
    position: fixed
    right: 0
    width: 100px
    height: 100px
    background: tomato
</style>