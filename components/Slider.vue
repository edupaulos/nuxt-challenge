<template>
  <div class="slider">
    <div
      class="slides-container"
      :style="{ transform: 'translateX(' + offset + ')' }"
    >
      <div
        v-for="(item, index) in sliderItems"
        :key="index"
        class="slider-item"
      >
        {{ item }}
      </div>
    </div>
    <button class="prev" @click="moveSlide(-1)">
      <img class="chevron" src="@/assets/icons/chevron-left.svg" />
    </button>
    <button class="next" @click="moveSlide(1)">
      <img class="chevron" src="@/assets/icons/chevron-right.svg" />
    </button>
  </div>
</template>

<script lang="ts" setup>
const sliderItems = ["Slide 1", "Slide 2", "Slide 3"];
let currentSlide = ref(0);

function moveSlide(direction: number) {
  const totalSlides = sliderItems.length;
  currentSlide.value =
    (currentSlide.value + direction + totalSlides) % totalSlides;
}

const offset = computed(() => -currentSlide.value * 100 + "%");
</script>

<style scoped lang="sass">
.slider
  position: relative
  overflow: hidden
  max-width: 1200px
  margin: 1rem auto

  .slides-container
    display: flex
    transition: transform 0.5s ease

  .slider-item
    flex: 0 0 calc(100% - 20rem)
    text-align: center
    padding: 10rem
    background-color: #E5E5E5

  .prev,
  .next
    position: absolute
    top: 35%
    padding: 1rem

    .chevron
      width: 5rem
      height: 5rem

  .prev
    left: 0

  .next
    right: 0
</style>
