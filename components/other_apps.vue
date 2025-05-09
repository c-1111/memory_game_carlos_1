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
  // Draggable.create("#app3", {
  //   type: 'x, y',
  //   dragClickables: true,
  // })
})
</script>

<template>
    <div class="app1" id="app1">
      <img src="/public/img/console2.png" alt="">
      <p style="text-align: center;">SUPER_FUN_<br>STUFF...</p>
    </div>
    <div class="app2" id="app2">
      <img src="/public/img/disk1.png" alt="">
      <p>doNotOpen</p>
    </div>
    <!-- <div class="app3" id="app3">
      <img src="/public/img/folder1.png" alt="">
      <p>Programa 1</p>
    </div> -->
</template>

<style scoped lang="sass">
p
  font-family: Calibri
  font-weight: lighter
  color: white
  text-shadow: 2px 2px 4px #000000

.app1
  display: flex
  flex-direction: column
  align-items: center
  width: auto
  height: auto
  position: fixed
  left: 100px
  top: 250px

  img
    max-height: 30px
    margin-bottom: .3rem


.app2
  display: flex
  flex-direction: column
  align-items: center
  width: auto
  height: auto
  position: fixed
  left: 130px
  top: 30px
  img
    max-height: 50px
    margin-bottom: .3rem

    
// .app3
//   width: auto
//   height: auto
//   position: fixed
//   right: 500px
//   img
//     max-height: 50px
</style>