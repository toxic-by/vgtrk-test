<script setup>
import 'vue3-carousel/dist/carousel.css'
import { ref } from 'vue';
import { Carousel, Slide } from 'vue3-carousel'
import PersonItem from './PersonItem.vue'

defineProps({
  persons: {
    type: Array,
    required: true
  }
})

const config = {
  itemsToShow: 8,
};

const breakpoints = {
  200: {
    itemsToShow: 2,
  },
  400: {
    itemsToShow: 3,
  },
  700: {
    itemsToShow: 4,
  },
  1024: {
    itemsToShow: 6,
  },
  1200: {
    itemsToShow: 8,
  },
}

const carouselRef = ref();

const next = () => {
  carouselRef.value.next()
  carouselRef.value.updateSlideWidth()
};
const prev = () => carouselRef.value.prev();
</script>

<template>
  <div class="persons-list">
    <Carousel ref="carouselRef" v-bind="config" :breakpoints="breakpoints">
      <Slide v-for="person in persons" :key="person.id">
        <PersonItem  :person="person" @click="$emit('showModal', person)"/>
      </Slide>

      <!-- <template #addons>
        <Navigation />
      </template> -->
    </Carousel>
    <div class="carousel-nav">
      <button class="carousel-nav_prev" @click="prev">
        <span class="carousel-nav_arrow carousel-nav_arrow__prev" />
      </button>
      <button class="carousel-nav_next" @click="next">
        <span class="carousel-nav_arrow carousel-nav_arrow__next" />
      </button>
    </div>
  </div>
</template>

<style >
.carousel, .carousel__viewport {
  width: 100%;
}

.persons-list {
  width: 100%;
  max-width: 1300px;
  position: relative;
  display: flex;
  flex-direction: row;
  justify-content: center;
  padding: 0 20px;
}

.carousel-nav_prev,
.carousel-nav_next {
  position: absolute;
  top: 25%;
  background: white;
	color: inherit;
	border: none;
	padding: 0;
	font: inherit;
	cursor: pointer;
	outline: inherit;
  border-radius: 50%;
  width: 48px;
  height: 48px;
  box-shadow: 0px 4px 20px 0px #0000000D;
}

.carousel-nav_prev {
  left: 0;
}

.carousel-nav_next {
  right: 0;
}

.carousel-nav_arrow {
  position: relative;
  display: inline-block;
  width: 15px;
  height: 2px;
  margin: 5px 0;
  border-radius: 9999px;
  background-color: #000000;
}

.carousel-nav_arrow::before,
.carousel-nav_arrow::after {
  content: "";
  position: absolute;
  top: calc(50% - 1px);
  width: 9px;
  height: 2px;
  border-radius: 9999px;
  background-color: #000000;
}

.carousel-nav_arrow::before {
  transform: rotate(45deg);
}

.carousel-nav_arrow::after {
  transform: rotate(-45deg);
}

.carousel-nav_arrow__prev::before,
.carousel-nav_arrow__prev::after  {
  left: 0;
  transform-origin: 1px 50%;
}

.carousel-nav_arrow__next::before,
.carousel-nav_arrow__next::after {
  right: 0;
  transform-origin: calc(100% - 1px) 50%;
}
</style>
