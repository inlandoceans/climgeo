<template>
  <div class="py-5">
    Specials
    <ul class="flex flex-wrap">
      <li
        v-for="special of specials"
        :key="special.slug"
        class="xs:w-full md:w-1/2 px-2 xs:mb-6 md:mb-12"
      >
        <img
          v-if="special.img"
          class="h-48 xxlmin:w-1/2 xxlmax:w-full object-cover"
          :src="special.img"
        />

        <div
          class="p-6 flex flex-col justify-between xxlmin:w-1/2 xxlmax:w-full"
        >
          <h2 class="font-bold">{{ special.title }}</h2>
          <p>by {{ special.author.name }}</p>
          <p class="font-bold text-gray-600 text-sm">
            {{ special.description }}
          </p>
        </div>
      </li>
    </ul>
    Features
    <ul class="flex flex-wrap">
      <li
        v-for="feature of features"
        :key="feature.slug"
        class="xs:w-full md:w-1/2 px-2 xs:mb-6 md:mb-12"
      >
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
            <p class="font-bold text-gray-600 text-sm">
              {{ feature.description }}
            </p>
          </div>
        </NuxtLink>
      </li>
    </ul>
    Articles
    <ul class="flex flex-wrap">
      <li
        v-for="article of articles"
        :key="article.slug"
        class="xs:w-full md:w-1/2 px-2 xs:mb-6 md:mb-12 article-card"
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
      .only(['title', 'description', 'img', 'slug', 'author'])
      .sortBy('createdAt', 'desc')
      .fetch()
    const specials = await $content('articles', params.slug)
      .where({ tags: { $contains: 'feature' } })
      .sortBy('createdAt', 'asc')
      .fetch()
    const features = await $content('features', params.slug)
      .only(['title', 'description', 'img', 'slug', 'author'])
      .sortBy('createdAt', 'desc')
      .fetch()
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
      specials,
      features,
      regions,
      tags
    }
  }
}
</script>

<style class="postcss">
.article-card {
  border-radius: 8px;
}
.article-card a {
  background-color: #fff;
  border-radius: 8px;
}
.article-card img div {
  border-radius: 8px 0 0 8px;
}
</style>
