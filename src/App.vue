<!--This project is using Axios to make HTTP requests-->

<template>
  <div>
    <GitubReq/>
  </div>
</template>

<script>
import GitubReq from './components/githubAPIComp.vue'

export default {
  name: 'App',
  components: {
    GitubReq
  }
}

</script>