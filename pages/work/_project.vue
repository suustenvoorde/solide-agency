<template>
  <div class="container">
    <div class="inner">
      <div class="project-detail" v-for="project in this.projects">
        <div class="project-subtitle">
          <div>
            <p class="client caption service">{{project.client}}</p>
            <p class="service caption service" v-for="service in project.services">{{service.title.value}}</p>
          </div>
          <h1>{{project.fulltitle}}</h1>
          <div v-html="project.context"></div>
          <img class="first-image" v-for="image in project.productImages" v-if="image.type.includes('image')" :alt="(image.description)" :src="image.url"/>
          <video v-for="image in project.productImages" v-if="image.type.includes('video')" controls>
            <source :src="image.url" :type="image.type">
          </video>
        </div>

        <div class="chapter" v-for="chapter in project.chapters">
          <h2>{{chapter.title.value}}</h2>
          <p class="small-text" v-html="chapter.text.value"></p>
          <img v-for="image in chapter.assets.value" v-if="image.type.includes('image')" :alt="(image.description)" :src="image.url"/>
          <video class="videos" v-for="image in chapter.assets.value" v-if="image.type.includes('video')" autoplay loop muted>
            <source :src="image.url" :type="image.type">
          </video>

        </div>

        <div class="meta">
          <div>
            <h2>Client</h2>
            <p class="small-text">{{project.client}}</p>
          </div>
          <div>
            <h2>Publication</h2>
            <p class="small-text date"> {{ project.date.toLocaleString("default", { month: "long" }) + ' '  + project.date.getFullYear() }}</p>
          </div>
          <div>
            <h2>Deliverables</h2>
            <p class="small-text" v-for="product in project.products">{{product.title.value}}</p>
          </div>
          <div v-if="project.websiteUrl">
            <h2>Link</h2>
            <a class="small-text cta-link" target="_blank" :href="'https://' + project.websiteUrl">{{project.websiteUrl}}</a>
            <a class="small-text cta-link" target="_blank" v-if="project.websiteUrl2" :href="'https://' + project.websiteUrl2">{{project.websiteUrl2}}</a>
          </div>
        </div>
      </div>
      <otherprojects limit="2"/>
    </div>
  </div>
</template>

<script>
import otherprojects from '~/components/other-projects.vue'

export default {
  transition: {
    css: false
  },
  components: {
    otherprojects,
  },
  async fetch ({store, params}) {
    return Promise.all([
      store.dispatch("company/getCompanyDetails"),
      store.dispatch("socialmedia/getSocialMediaAccounts")
    ])
  },
  validate ({ params, store, context}) {
    // Check if `params.id` is an existing category
      return store.state.projectSlug.some(project =>
        project.slug === params.project
      )
  },
  computed: {
    projects: function(){
      return this.$store.state.projectSlug.filter(project =>
        project.slug === this.$route.params.project
      )
    }
  },
  mounted(){
    var videos = document.querySelectorAll('video')
    function myFunction(x) {
      if (x.matches) { // If media query matches
        videos.forEach(function(e){
          e.pause();
        })
      } else {
        videos.forEach(function(e){
          e.play();
        })
      }
    }

    var x = window.matchMedia("(max-width: 700px)")
    myFunction(x) // Call listener function at run time
    x.addListener(myFunction) // Attach listener function on state changes
  }
}
</script>
