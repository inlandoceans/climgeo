<template>
  <div class="mx-5 py-5">
    <div>
      <div class="container grid md:grid-cols-12 mb-2 mx-auto">
        <div class="md:col-span-8">
          <NuxtLink
            :to="{ name: 'feature-slug', params: { slug: featuredPost.slug } }"
            class="col-span-3 transition-shadow duration-150 ease-in-out shadow-sm hover:shadow-md"
          >
            <img v-if="featuredPost.img" :src="`${featuredImg}`" />
          </NuxtLink>
        </div>
        <div class="mx-8 px-5 md:col-span-4 py-5">
          <NuxtLink
            :to="{ name: 'feature-slug', params: { slug: featuredPost.slug } }"
          >
            <h2 class="sm:text-2xl lg:text-4xl font-extrabold">
              {{ featuredPost.title }}
            </h2>
            <span class="mr-3">
              {{ formatDate(featuredPost.createdAt) }}
            </span>
            <p class="text-sm font-spectral text-red-900">
              by {{ featuredPost.author.name }}
            </p>
            <p class="font-spectral text-md py-5">
              {{ featuredPost.description }}
            </p>
          </NuxtLink>
        </div>
      </div>
      <div
        class="mx-12 my-4 h-1 bg-gradient-to-r from-blue-800 to-red-800"
      ></div>
      <div>
        <ul class="flex flex-wrap">
          <li
            v-for="article of articles"
            :key="article.slug"
            class="xs:w-full md:w-1/2 px-2 xs:mb-6 md:mb-12"
          >
            <NuxtLink
              :to="{ name: 'blog-slug', params: { slug: article.slug } }"
              class="flex transition-shadow duration-150 ease-in-out shadow-sm hover:shadow-md xxlmax:flex-col"
            >
              <img
                v-if="article.img"
                class="h-48 xxlmin:w-1/2 xxlmax:w-full object-cover"
                :src="article.img"
              />

              <div
                class="p-6 flex flex-col justify-between xxlmin:w-1/2 xxlmax:w-full"
              >
                <h2 class="font-bold">{{ article.title }}</h2>
                <p>by {{ article.author.name }}</p>
                <p class="font-bold text-gray-600 text-sm">
                  {{ article.description }}
                </p>
              </div>
            </NuxtLink>
          </li>
        </ul>
      </div>

      <div
        class="container grid grid-cols-3 gap-2 border-t border-solid border-grey-700"
      >
        <div v-for="feature of features" :key="feature.slug" class="col-span-1">
          <NuxtLink
            :to="{ name: 'feature-slug', params: { slug: feature.slug } }"
            class="flex transition-shadow duration-150 ease-in-out shadow-sm hover:shadow-md xxlmax:flex-col"
          >
            <img
              v-if="feature.img"
              class="h-48 xxlmin:w-1/2 xxlmax:w-full object-cover"
              :src="feature.img"
            />

            <div
              class="p-6 flex flex-col justify-between xxlmin:w-1/2 xxlmax:w-full"
            >
              <h2 class="font-bold">{{ feature.title }}</h2>
              <p>by {{ feature.author.name }}</p>
              <p class="font-spectral">
                {{ feature.description }}
              </p>
            </div>
          </NuxtLink>
        </div>
      </div>
    </div>

    <h3 class="mb-4 font-bold text-2xl uppercase text-center">Topics</h3>
    <ul class="flex flex-wrap mb-4 text-center">
      <li
        v-for="tag of tags"
        :key="tag.slug"
        class="xs:w-full md:w-1/3 lg:flex-1 px-2 text-center"
      >
        <NuxtLink :to="`/tag/${tag.slug}`" class="">
          <p class="font-bold text-gray-600 uppercase tracking-wider text-ss">
            {{ tag.name }}
          </p>
        </NuxtLink>
      </li>
    </ul>
    <h3 class="mb-4 font-bold text-2xl uppercase text-center">Regions</h3>
    <ul class="flex flex-wrap mb-4 text-center">
      <li
        v-for="region of regions"
        :key="region.slug"
        class="xs:w-full md:w-1/3 lg:flex-1 px-2 text-center"
      >
        <NuxtLink :to="`/region/${region.slug}`" class="">
          <p class="font-bold text-gray-600 uppercase tracking-wider text-ss">
            {{ region.name }}
          </p>
        </NuxtLink>
      </li>
    </ul>

    <footer class="flex justify-center border-gray-500 border-t-2">
      <p class="mt-4">
        Created by
        <a
          href="https://twitter.com/debs_obrien"
          class="font-bold hover:underline"
          >Debbie O'Brien</a
        >
        at NuxtJS. See the
        <a
          href="https://nuxtjs.org/blog/creating-blog-with-nuxt-content"
          class="font-bold hover:underline"
          >tutorial</a
        >
        for how to build it.
      </p>
    </footer>
  </div>
</template>

<script>
export default {
  async asyncData({ $content, params }) {
    const articles = await $content('articles', params.slug)
      .where({ category: { $contains: 'article' } })
      .only(['title', 'description', 'img', 'slug', 'author'])
      .sortBy('createdAt', 'desc')
      .fetch()
    const tempfeatures = await $content('articles', params.slug)
      .where({ category: { $contains: 'feature' } })
      .only([
        'title',
        'description',
        'img',
        'slug',
        'author',
        'createdAt',
        'updatedAt'
      ])
      .sortBy('createdAt', 'desc')
      .fetch()
    const featuredPost = tempfeatures[0]
    const featuredImg = require(`../assets/images/${featuredPost.img}`)
    const features = tempfeatures.slice(1, tempfeatures.length)

    const regions = await $content('regions', params.slug)
      .only(['name', 'description', 'img', 'slug'])
      .sortBy('createdAt', 'desc')
      .fetch()
    const tags = await $content('tags', params.slug)
      .only(['name', 'description', 'img', 'slug'])
      .sortBy('createdAt', 'asc')
      .fetch()
    return {
      articles,
      features,
      featuredImg,
      featuredPost,
      regions,
      tags
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

<style class="postcss">
.article-card a {
  background-color: #fff;
  border-radius: 8px;
}
.article-card img div {
  border-radius: 8px 0 0 8px;
}
</style>
