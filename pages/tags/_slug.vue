<template>
  <div>
    <!-- <h1>{{ tag.fields.name }}</h1> -->
  </div>
</template>

<script>
export default {
  asyncData({ payload, params, error, store, env }) {
    let tag = payload
    console.log('tag', tag)
    if (!tag) {
      for (let i = 0; i < store.state.posts.length; i++) {
        const tags = store.state.posts[i].fields.tags
        if (tags) tag = tags.find(tag => tag.fields.slug === params.slug)
        if (tag) break
      }
    }
    if (tag) {
      return { tag }
    } else {
      error({ statusCode: 400 })
    }
  }
}
</script>