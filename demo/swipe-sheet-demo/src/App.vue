<script setup lang="ts">
import { ref } from "vue";
import { SwipeSheet } from "vue3-swipe-sheet";
import "vue3-swipe-sheet/dist/style.css";

const lastEmit = ref<string>("")
const showSheet = ref(true)
const closeOnDraggedDown = ref(false)
const closeOnClickOutside = ref(false)
const sheetRatio = ref<number>(0.9)

const handleClickedOutside = () => {
  lastEmit.value = "clicked outside"
  if (closeOnClickOutside.value) {
    showSheet.value = false
  }

}

const handleDraggedDown = () => {
  lastEmit.value = "dragged down"
  if (closeOnDraggedDown.value) {
    showSheet.value = false
  }

}

</script>

<template>
  <div class="flex-container">
    <div class="controls">
      <p>Last emit:{{ lastEmit }}</p>
      <div> <label for="close-on-drag">close on drag down</label>
        <input name="close-on-drag" type="checkbox" v-model="closeOnDraggedDown">
      </div>

      <div> <label for="close-on-click-outside">close on click outside</label>
        <input name="close-on-click-outside" type="checkbox" v-model="closeOnClickOutside">
      </div>
      <div><label for="ratio-select">select ratio of sheet to container</label><input name="ratio-select"
          style="width: fit-content;" type="number" step="0.1" v-model="sheetRatio" min="0" max="1"></div>
      <button style="width: fit-content;" @click="showSheet = !showSheet">toggle sheet</button>
    </div>
    <div class="sheet">
      <SwipeSheet v-if="showSheet" :ratioSheetHeight="sheetRatio" @clicked-outside="handleClickedOutside"
        @expanded="lastEmit = 'expanded'" @minimized="lastEmit = 'minimized'" @draggedDown="handleDraggedDown"
        style="font-size: 1.5rem; ">
        <template v-slot:header>
          <div class="header"> <svg height="8" width="200" xmlns="http://www.w3.org/2000/svg">
              <line x1="0" y1="10" x2="200" y2="10" style="stroke:#A4A4A4;stroke-width:8;" />
            </svg>
            <p>testlorem Lorem ipsum dolor sit amet consectetur adipisicing elit. A placeat libero dolorem commodi fugit
              tempora adipisci et tenetur, perspiciatis vitae nihil sapiente cumque molestiae nesciunt ducimus quia
              itaque
              animi nostrum!</p>
          </div>

        </template>
        <template v-slot:content>
          <div class="content">
            <p>Lorem ipsum, dolor sit amet consectetur adipisicing elit. Eligendi iure debitis praesentium aliquid, hic
              minima neque distinctio consequuntur! Culpa, voluptatum id eaque delectus numquam itaque quis omnis a
              atque
              illum?Lorem ipsum, dolor sit amet consectetur adipisicing elit. Eligendi iure debitis praesentium aliquid,
              hic
              minima neque distinctio consequuntur! Culpa, voluptatum id eaque delectus numquam itaque quis omnis a
              atque
              illum?Lorem ipsum, dolor sit amet consectetur adipisicing elit. Eligendi iure debitis praesentium aliquid,
              hic
              minima neque distinctio consequuntur! Culpa, voluptatum id eaque delectus numquam itaque quis omnis a
              atque
              illum?Lorem ipsum, dolor sit amet consectetur adipisicing elit. Eligendi iure debitis praesentium aliquid,
              hic
              minima neque distinctio consequuntur! Culpa, voluptatum id eaque delectus numquam itaque quis omnis a
              atque
              illum?Lorem ipsum, dolor sit amet consectetur adipisicing elit. Eligendi iure debitis praesentium aliquid,
              hic
              minima neque distinctio consequuntur! Culpa, voluptatum id eaque delectus numquam itaque quis omnis a
              atque
              illum?Lorem ipsum, dolor sit amet consectetur adipisicing elit. Eligendi iure debitis praesentium aliquid,
              hic
              minima neque distinctio consequuntur! Culpa, voluptatum id eaque delectus numquam itaque quis omnis a
              atque
              illum?Lorem ipsum, dolor sit amet consectetur adipisicing elit. Eligendi iure debitis praesentium aliquid,
              hic
              minima neque distinctio consequuntur! Culpa, voluptatum id eaque delectus numquam itaque quis omnis a
              atque
              illum?Lorem ipsum, dolor sit amet consectetur adipisicing elit. Eligendi iure debitis praesentium aliquid,
              hic
              minima neque distinctio consequuntur! Culpa, voluptatum id eaque delectus numquam itaque quis omnis a
              atque
              illum?inima neque distinctio consequuntur! Culpa, voluptatum id eaque delectus numquam itaque quis omnis a
              atque
              illum?Lorem ipsum, dolor sit amet consectetur adipisicing elit. Eligendi iure debitis praesentium aliquid,
              hic
              minima neque distinctio consequuntur! Culpa, voluptatum id eaque delectus numquam itaque quis omnis a
              atque
              illum?</p>
          </div>

        </template>
      </SwipeSheet>
    </div>

  </div>

</template>

<style scoped>
.flex-container {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: space-evenly;
  gap: 5rem;
  height: 90vh;
  padding: 1rem;

}

.flex-container>* {
  min-width: 300px;
}

.controls {
  display: flex;
  flex-direction: column;
  flex-wrap: nowrap;

}

.sheet {
  border-style: solid;
  border-color: black;
  height: 100%;
  width: 33%;
}

.header {
  display: flex;
  flex-direction: column;
  flex-wrap: nowrap;
  justify-content: center;
  align-items: center;
  padding: 1rem;
  background-color: lightgray;
  border-radius: 25px 25px 0px 0px;
}

.content {
  padding: 0.5rem;
  background-color: whitesmoke;
}
</style>
