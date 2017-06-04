<template>
  <q-layout>
  <div slot="header" class="toolbar blue">
    <!-- opens drawer using its ref -->
    <button class="hide-on-drawer-visible" @click="$refs.drawer.open()">
      <i></i>
    </button>
    <q-toolbar-title :padding="2">
      {{ $data.title }}
    </q-toolbar-title>
    <button>
      <i>alarm</i>
    </button>
  </div>

    <div class="layout-view">
      <div class="card" v-for="post in $data.posts">
        <div class="card-title">
          <h5>{{ post.title }} <small>[{{ post.sentiment }}]</small></h5>
          <p class="light-paragraph">By <a :href="$data.baseUrl">{{ post.category_id }}</a></p>
        </div>
        <q-parallax v-if="post.image" :src="$data.baseUrl + post.image" class="responsive">
        </q-parallax>
        <div class="card-content">
          <p class="light-paragraph">{{ post.summary }}</p>
        </div>
        <div class="card-actions">
          <div class="text-primary">
            <i>thumb_up</i> <a :href="post.url">Read full</a>
          </div>
          <div class="auto"></div>
          <div class="text-grey-6">
            {{ post.date | formatDate }}
          </div>
        </div>
      </div>
    </div>
  </q-layout>
</template>

<script>
import axios from 'axios'

export default {
  data () {
    return {
      posts: this.posts,
      baseUrl: 'https://qprob.com/',
      title: 'Trading Today'
    }
  },
  methods: {
    fetchData () {
      axios.get('https://api.qprob.com/posts/').then(response => {
        this.posts = response.data.data
      }).catch(e => {
        console.log(e)
      })
    }
  },
  mounted () {
    this.fetchData()
  }
}
</script>

<link href="./node_modules/progressive-image/dist/index.css" rel="stylesheet" type="text/css">
