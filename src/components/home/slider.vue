<template>
  <div class='relative'>

    <div class='absolute inset-x-0 bottom-0 h-1/2 bg-accent-color-50' />
    <SliderBackground />
    <Loader v-if='slides.length === 0' height='h-48 sm:h-96' />
    <div v-else class='mx-auto max-w-7xl h-48 sm:h-96 sm:px-6 lg:px-8'>
      <carousel :autoplay='5000' :items-to-show='1' :wrap-around='true'>
        <slide v-for='slide in slides' :key='slide' class='relative w-full shadow-xl sm:overflow-hidden'>
          <div class='absolute inset-0'>
            <img :src='this.$filters.getFirstMediaUrl(slide)'
                 alt='People working on laptops'
                 class='object-cover w-full h-full' />
            <div class='absolute inset-0' />
          </div>
          <div class='flex relative items-center px-6 w-full h-48 sm:px-10 sm:h-96'>
            <p class='w-1/3 text-sm font-bold text-gray-700 sm:text-xl lg:text-3xl'>
              {{ this.$filters.transString(slide.text) }}
            </p>
          </div>
        </slide>
        <template #addons>
          <navigation v-if='slides.length > 0' class='hidden sm:flex' />
        </template>
      </carousel>
    </div>
    <SearchBar />
  </div>
</template>
<style>
@media (min-width: 640px) {
  .carousel .carousel__viewport {
    border-radius: 1rem;
  }
}
</style>
<script>
import 'vue3-carousel/dist/carousel.css'
import { Carousel, Navigation, Slide } from 'vue3-carousel'
import { createNamespacedHelpers } from 'vuex'
import SliderBackground from './partial/slider_background.vue'
import SearchBar from './partial/search_bar.vue'
import Loader from '../partial/loader.vue'

const { mapState, mapActions } = createNamespacedHelpers('slider')

export default {
  name: 'Slider',
  components: {
    Loader,
    Carousel,
    Slide,
    Navigation,
    SliderBackground,
    SearchBar,
  },
  mounted() {
    this.getSlidesAction()
  },
  computed: {
    ...mapState(['slides']),
  },
  methods: {
    ...mapActions(['getSlidesAction']),
  },
}
</script>