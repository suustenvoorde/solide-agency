<template>
  <div class="container">
    <div class="inner">
      <keyvalue page="Approach"/>
      <principle/>
      <h1 class="process-title">Our process</h1>
      <processStep/>
      <service disable="Prepaid cards" pagename="Services"/>
      <calltoaction linkto="Contact"/>
    </div>
  </div>
</template>

<script>
import keyvalue from '~/components/keyvalue.vue'
import processStep from '~/components/processStep.vue'
import principle from '~/components/principle.vue'
import service from '~/components/service.vue'
import calltoaction from '~/components/calltoaction.vue'


export default {
  transition: {
    css: false
  }, // set our transition with nuxt.js
  components: {
    keyvalue,
    processStep,
    principle,
    service,
    calltoaction
  },
  async fetch ({store, params}) {
    return Promise.all([
      store.dispatch("company/getCompanyDetails"),
      store.dispatch("socialmedia/getSocialMediaAccounts"),
      store.dispatch("keyvalue/getKeyValues"),
      store.dispatch("process_step/getOurProcessSteps"),
      store.dispatch("principle/getPrinciples"),
      store.dispatch("service/getServices"),
      store.dispatch("calltoaction/getCallToActions")
    ])
  },
  head () {
    return {
      // meta: [
      //   { hid: 'description', name: 'description', content: this.$store.state.teasers.metadescription },
      //   { name: 'keywords', content: this.$store.state.teasers.metadekeywords }
      // ]
    }
  }
}
</script>
