<template>
  <div>
    <div class="column is-half" v-for="(post, index) in posts" :key="index">
      <div class="card">
        <header class="card-header">
          <p class="card-header-title">{{ post.fields.title }}</p>
        </header>
        <div class="card-content">
          <div class="content">
            {{ post.fields.description }}
            <br />
            <small>{{ ( new Date(post.fields.publishDate)).toDateString() }}</small>
            <br />
            <nuxt-link
              v-for="(tag, index) in post.fields.tags"
              :key="index"
              :to="{ names: 'tags', params: `${tag.sys.id}`, path: `/tags/${tag.sys.id}/`}"
            >{{ tag.fields.name }}</nuxt-link>
          </div>
        </div>
        <footer class="card-footer">
          <nuxt-link
            :to="{ name: 'id', params: { id: post.sys.id }}"
            class="card-footer-item"
          >Read More</nuxt-link>
        </footer>
      </div>
    </div>
  </div>
</template>

<script>
import { createClient } from "~/plugins/contentful.js";
const client = createClient();
export default {
  head: {
    title: "Tag"
  },
  data() {
    return {
      posts: [],
      tag_id: this.$route.params.id
    };
  },
  asyncData({ env, params }) {
    return client
      .getEntries({
        // fetch all blog posts sorted by creation date
        content_type: env.CTF_BLOG_POST_TYPE_ID,
        "fields.tags.sys.id": params.id,
        order: "-fields.publishDate"
      })
      .then(entries => {
        return {
          posts: entries.items
        };
      })
      .catch(console.error);
  },
  methods: {}
};
</script>

<style scoped>
.card {
  width: 300px;
  height: 200px;
  box-shadow: 1px 2px 3px 1px rgba(0, 0, 0, 0.2);
  border: 0.5px solid rgb(57, 72, 85);
  padding: 10px 20px;
  margin: 10px 10px;
}
.card_title {
  font-size: 1.2rem;
}
.card_text {
  color: rgb(57, 72, 85);
  margin: 10px 0;
}
.card_date {
  font-size: 0.7rem;
  color: rgb(57, 72, 85);
  text-align: right;
}
</style>
