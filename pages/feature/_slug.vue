<template>
  <article class="mx-auto">
    <div class="py-8 text-center">
      <img :src="feature.img" :alt="feature.alt" class="" />

      <h1 class="text-4xl md:text-6xl font-bold">{{ feature.title }}</h1>
      <span v-for="(tag, id) in feature.tags" :key="id">
        <NuxtLink :to="`/tag/${tags[tag].slug}`">
          <span
            class="truncate uppercase tracking-wider font-medium text-sm px-2 py-1 rounded mr-2 mb-2 border border-light-border dark:border-dark-border transition-colors duration-300 ease-linear"
          >
            {{ tags[tag].name }}
          </span>
        </NuxtLink>
      </span>
      <p class="py-2 text-lg">{{ feature.description }}</p>
      <div class="mx-24 h-1 bg-gradient-to-r from-blue-800 to-red-800"></div>
      <div class="text-center mt-8 uppercase text-sm">
        <span class="mr-3">
          {{ formatDate(feature.updatedAt) }}
        </span>
        <span class="mr-3">•</span>
        <NuxtLink :to="`/author/${feature.author.name}`">
          <span>{{ feature.author.name }}</span>
        </NuxtLink>
      </div>
    </div>
    <div class="max-w-2xl mx-auto h-full px-6">
      <!-- table of contents -->
      <nav class="hidden pb-6">
        <ul>
          <li
            v-for="link of feature.toc"
            :key="link.id"
            :class="{
              'font-semibold': link.depth === 2
            }"
          >
            <nuxtLink
              :to="`#${link.id}`"
              class="hover:underline"
              :class="{
                'py-2': link.depth === 2,
                'ml-2 pb-2': link.depth === 3
              }"
              >{{ link.text }}</nuxtLink
            >
          </li>
        </ul>
      </nav>
      <!-- content from markdown -->
      <nuxt-content :document="feature" max-w-2xl />
      <p class="pb-4">Post last updated: {{ formatDate(feature.updatedAt) }}</p>
      <!-- content author component -->
      <author :author="feature.author" />
      <!-- prevNext component -->
      <PrevNextFeat :prev="prev" :next="next" class="mt-8" />
    </div>
  </article>
</template>
<script>
export default {
  async asyncData({ $content, params }) {
    const feature = await $content('articles', params.slug).fetch()
    const tagsList = await $content('tags')
      .only(['name', 'slug'])
      .where({ name: { $containsAny: feature.tags } })
      .fetch()
    const tags = Object.assign({}, ...tagsList.map((s) => ({ [s.name]: s })))
    const [prev, next] = await $content('articles')
      .where({ category: { $contains: 'feature' } })
      .only(['title', 'slug'])
      .sortBy('createdAt', 'asc')
      .surround(params.slug)
      .fetch()
    return {
      feature,
      tags,
      prev,
      next
    }
  },
  methods: {
    formatDate(date) {
      const options = { year: 'numeric', month: 'long', day: 'numeric' }
      return new Date(date).toLocaleDateString('en', options)
    }
  }
}
</script>
<style>
.nuxt-content p {
  margin-bottom: 20px;
  font-family: Georgia, 'Times New Roman', Times, serif;
}
.nuxt-content h2 {
  font-weight: bold;
  font-size: 28px;
  font-family: Georgia, 'Times New Roman', Times, serif;
}
.nuxt-content h3 {
  font-weight: bold;
  font-size: 22px;
}
</style>
