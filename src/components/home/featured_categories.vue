<template>

  <div class='py-10 bg-accent-color-50'>

    <!-- Offline Services -->
    <div class='flex justify-between items-center px-4 py-10 mx-auto max-w-7xl sm:px-6 lg:px-8'>
      <div class='flex-1 min-w-0'>
        <h2 class='py-3 text-xl font-bold lg:text-4xl text-second-color-600'>
          {{ $t('Offline Categories') }}
        </h2>
        <p class='text-sm sm:text-base text-second-color-400 sm:truncate'>
          {{ $t('Featured Categories Subtitle') }}
        </p>
      </div>
      <div class='flex mt-4 md:mt-0'>
        <router-link :to='{name:"Categories"}'
                     class='inline-flex items-center px-4 py-2 ml-3 text-sm font-medium text-white rounded-md border border-transparent shadow-sm bg-main-color-600 hover:bg-main-color-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-main-color-500'
                     type='button'>
          {{ $t('View All') }}
        </router-link>
      </div>
    </div>

    <div class='grid overflow-hidden grid-cols-2 gap-4 px-4 mx-auto max-w-7xl sm:py-6 sm:px-6 lg:px-8 sm:grid-cols-3 lg:grid-cols-6 sm:gap-8'>
      <CategoryItem v-for='category in offlineCategories' :key='category.id' :category='category' />
    </div>

    <!-- Online Services -->
    <div class='flex justify-between items-center px-4 py-10 mx-auto max-w-7xl sm:px-6 lg:px-8'>
      <div class='flex-1 min-w-0'>
        <h2 class='py-3 text-xl font-bold lg:text-4xl text-second-color-600'>
          {{ $t('Online Categories') }}
        </h2>
        <p class='text-sm sm:text-base text-second-color-400 sm:truncate'>
          {{ $t('Featured Categories Subtitle') }}
        </p>
      </div>
      <div class='flex mt-4 md:mt-0'>
        <router-link :to='{name:"Categories"}'
                     class='inline-flex items-center px-4 py-2 ml-3 text-sm font-medium text-white rounded-md border border-transparent shadow-sm bg-main-color-600 hover:bg-main-color-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-main-color-500'
                     type='button'>
          {{ $t('View All') }}
        </router-link>
      </div>
    </div>

    <div class='grid overflow-hidden grid-cols-2 gap-4 px-4 mx-auto max-w-7xl sm:py-6 sm:px-6 lg:px-8 sm:grid-cols-3 lg:grid-cols-6 sm:gap-8'>
      <CategoryItem v-for='category in onlineCategories' :key='category.id' :category='category' />
    </div>

  </div>

</template>

<script>

import { createNamespacedHelpers } from 'vuex'
import CategoryItem from './partial/category_item.vue'
import axios from 'axios'

const { mapState, mapActions } = createNamespacedHelpers('category')


export default {
  components: { CategoryItem },
  data: () => ({
    onlineCategories: [],
    offlineCategories: [],
  }),
  mounted() {
    this.getFeaturedCategoriesAction()

    axios.get("https://admin.allsolutioncompany.com/api/online-categories")
      .then((response) => {
        console.log(response.data);
        let i = 0;
        let length = response.data.data.length;
        for (i; i < length; i++) {
          this.onlineCategories.push(response.data.data[i]);
        }
      })
      .catch((error) => (this.errors = error.response.data.errors));

    axios.get("https://admin.allsolutioncompany.com/api/offline-categories")
      .then((response) => {
        console.log(response.data);
        let i = 0;
        let length = response.data.data.length;
        for (i; i < length; i++) {
          this.offlineCategories.push(response.data.data[i]);
        }
      })
      .catch((error) => (this.errors = error.response.data.errors));
  },
  computed: {
    ...mapState(['featuredCategories']),
  },
  methods: {
    ...mapActions(['getFeaturedCategoriesAction']),
  },
}
</script>