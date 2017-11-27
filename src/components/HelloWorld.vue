<template>
  <div class="hello">
    
    <template v-for="blok in story.content.body">
      <component  :blok="blok" :is="blok.component"></component>
    </template>

    <h2>Essential Links</h2>
    <ul>
      <li><a href="https://vuejs.org" target="_blank">Core Docs</a></li>
      <li><a href="https://forum.vuejs.org" target="_blank">Forum</a></li>
      <li><a href="https://chat.vuejs.org" target="_blank">Community Chat</a></li>
      <li><a href="https://twitter.com/vuejs" target="_blank">Twitter</a></li>
      <br>
      <li><a href="http://vuejs-templates.github.io/webpack/" target="_blank">Docs for This Template</a></li>
    </ul>
    <h2>Ecosystem</h2>
    <ul>
      <li><a href="http://router.vuejs.org/" target="_blank">vue-router</a></li>
      <li><a href="http://vuex.vuejs.org/" target="_blank">vuex</a></li>
      <li><a href="http://vue-loader.vuejs.org/" target="_blank">vue-loader</a></li>
      <li><a href="https://github.com/vuejs/awesome-vue" target="_blank">awesome-vue</a></li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
data () {
  return {
    msg: 'Welcome to Your Vue.js App',
    story: { content: { body: [] } }
  }
},
  created () {
    // initialize the Storyblok client 
    this.$storyblok.init({
      accessToken: 'YOUR_ACCESS_TOKEN'
    })

    // load either the draft or the
    // published version of your content
    // if you're in the editor or not.
    this.$storyblok.pingEditor(() => {
      if (this.$storyblok.isInEditor()) {
        this.getStory('draft')
      } else {
        this.getStory('published')
      }
    })
    
    // register on change event which will
    // be triggered as soon someone clicks on "save"
    // the Storyblok compose interface.
    this.$storyblok.on('change', () => {
      this.getStory('draft')
    })
  },
methods: {
  getStory (version) {
    this.$storyblok.get({
      slug: 'home', 
      version: version
    }, (data) => {
      // default value for story
      this.story = { content: { body: [] } }
      // on nextTick add the story information
      // to this.story
      this.$nextTick(() => {
        this.story = data.story
      })
    })
  }
}
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
