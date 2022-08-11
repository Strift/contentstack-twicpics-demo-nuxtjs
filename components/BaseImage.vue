<template>
  <TwicImg :src="imageUrl" :alt="alt" mode="cover" v-bind="$attrs"  />
</template>

<script lang="ts">
import Vue from 'vue'

export default Vue.extend({
  props: {
    src: {
      required: true,
      type: String
    },
    alt: {
      required: true,
      type: String
    }
  },
  computed: {
    imageUrl () {
      return this.src.replace("https://eu-images.contentstack.com/", "")
    }
  }
})
</script>