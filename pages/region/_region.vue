<template>
  <div
    class="flex lg:h-screen w-screen lg:overflow-hidden xs:flex-col lg:flex-row"
  >
    <div class="relative lg:w-1/2 xs:w-full xs:h-84 lg:h-full post-left">
      <img
        :src="region.img"
        :alt="region.name"
        class="absolute h-full w-full object-cover"
      />
    </div>

    <div class="relative lg:w-1/2 xs:w-full xs:h-84 lg:h-full post-left">
      <h1 class="text-4xl font-bold uppercase">
        {{ region.name }}
      </h1>
      <p class="mb-4 uppercase">{{ region.description }}</p>

      <nuxt-content :document="region" />
    </div>

    <div
      class="relative xs:py-8 xs:px-8 lg:py-32 lg:px-16 lg:w-1/2 xs:w-full h-full overflow-y-scroll markdown-body post-right custom-scroll"
    >
      <NuxtLink to="/"
        ><p class="hover:underline">Back to All Articles</p></NuxtLink
      >
      <h3 class="mb-4 font-bold text-4xl">
        Features tagged {{ region.name }}:
      </h3>
      <ul>
        <li
          v-for="feature in features"
          :key="feature.slug"
          class="w-full px-2 xs:mb-6 md:mb-12 article-card"
        >
          <NuxtLink
            :to="{ name: 'blog-slug', params: { slug: feature.slug } }"
            class="flex transition-shadow duration-150 ease-in-out shadow-sm hover:shadow-md xxlmax:flex-col"
          >
            <img
              v-if="feature.img"
              class="h-48 xxlmin:w-1/2 xxlmax:w-full object-cover"
              :src="feature.img"
              :alt="feature.alt"
            />

            <div
              class="p-6 flex flex-col justify-between xxlmin:w-1/2 xxlmax:w-full"
            >
              <h2 class="font-bold">{{ feature.title }}</h2>
              <p>{{ feature.description }}</p>
              <p class="font-bold text-gray-600 text-sm">
                {{ formatDate(feature.updatedAt) }}
              </p>
            </div>
          </NuxtLink>
        </li>
      </ul>
      <h3 class="mb-4 font-bold text-4xl">
        Articles tagged {{ region.name }}:
      </h3>
      <ul>
        <li
          v-for="article in articles"
          :key="article.slug"
          class="w-full px-2 xs:mb-6 md:mb-12 article-card"
        >
          <NuxtLink
            :to="{ name: 'blog-slug', params: { slug: article.slug } }"
            class="flex transition-shadow duration-150 ease-in-out shadow-sm hover:shadow-md xxlmax:flex-col"
          >
            <img
              v-if="article.img"
              class="h-48 xxlmin:w-1/2 xxlmax:w-full object-cover"
              :src="article.img"
              :alt="article.alt"
            />

            <div
              class="p-6 flex flex-col justify-between xxlmin:w-1/2 xxlmax:w-full"
            >
              <h2 class="font-bold">{{ article.title }}</h2>
              <p>{{ article.description }}</p>
              <p class="font-bold text-gray-600 text-sm">
                {{ formatDate(article.updatedAt) }}
              </p>
            </div>
          </NuxtLink>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  async asyncData({ $content, params }) {
    const regions = await $content('regions')
      .where({ slug: { $contains: params.region } })
      .limit(1)
      .fetch()
    const region = regions.length > 0 ? regions[0] : {}
    const articles = await $content('articles', params.slug)
      .where({ regions: { $contains: region.name } })
      .sortBy('createdAt', 'asc')
      .fetch()
    return {
      articles,
      region
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
