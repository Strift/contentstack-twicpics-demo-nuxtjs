<template>
  <main>
    <BlogBanner :data="bannerData" />
    <div class="authors-container">
      <div v-for="author in list" :key="author.uuid" class="author-card">
        <BaseImage 
          :src="author.picture.url" 
          :alt="author.bio"
          focus="auto" 
          class="author-image" 
        />
        <div>
          <h2>{{ author.title }}</h2>
          <p>{{ author.bio }}</p>
        </div>
      </div>
    </div>
  </main>
</template>

<script lang="ts">

import Stack from '../plugins/contentstack'
import PageData from '@/typescript/pages'
import BlogBanner from '@/components/BlogBanner.vue'

interface Author {
  uuid: string
  bio: string
  picture: {
    url: string
  }
  title: string
}

export default {
  components: {
    BlogBanner
  },
  async asyncData(req: PageData) {
    const page = await Stack.getEntryByUrl({
      contentTypeUid: 'page',
      entryUrl: `${req.route.path}`,
    })

    const list: Array<Author> = await Stack.getEntries({
      contentTypeUid: 'author',
      jsonRtePath: ['full_name', 'picture', 'bio'],
    })

    return {
      page: page[0],
      list
    }
  },
  data: () => ({
    bannerData: {
      bg_color: '#715cdd',
      banner_title: 'Authors',
      banner_description: 'Lorem ipsum dolor sit, amet consectetur adipisicing elit. Quos voluptatum temporibus laborum nisi illum repellendus nobis fugit maiores enim cumque odit neque corrupti non, aperiam nam quasi at, nemo beatae.'
    }
  }),
  mounted() {
    this.$store.commit('setPage', this.page)
    this.$store.commit('setBlogpost', null)
  },
}
</script>