<template>
  <div class='flex inset-x-0 items-center mx-8 bg-white rounded-full shadow-2xl transform -translate-y-full sm:mx-auto sm:w-3/4 md:w-1/2 lg:w-1/3'>
    <input :placeholder='$t("Search Anything")'
           :value='keywords'
           class='px-6 sm:w-42 w-full h-3 bg-white rounded-full border-none focus:outline-none focus:ring-0'
           type='text'
           @input='search'>
    <router-link :to='{name:"Search",params:{keywords:keywords}}' class='py-4 w-40 text-sm text-center text-white rounded-full bg-main-color-600'>
      {{ $t('Search') }}
    </router-link>
  </div>
</template>

<script>
import { mapState, mapActions } from 'vuex'

export default {
  name: 'SearchBar',
  computed: {
    ...mapState('eService', {
      keywords: state => state.keywords,
    }),
  },
  mounted() {
    this.updateKeywordsAction(undefined)
  },
  methods: {
    ...mapActions('eService', [
      'updateKeywordsAction',
    ]),
    search(e) {
      this.updateKeywordsAction(e.target.value)
    },
  },
}

</script>