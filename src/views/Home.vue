<template>
  <div class="home">
    <NavBar/>
    <div class="col-12 mt-3" style="min-height: calc(100vh - 128px)">
      <WebSocket/>
    </div>
    <ContentFooter/>
  </div>
</template>

<script>
// @ is an alias to /src
import NavBar from '@/components/NavBar.vue'
import WebSocket from '@/components/WebSocket.vue'
import ContentFooter from '@/components/ContentFooter.vue'

export default {
  name: 'Home',
  components: {
    NavBar,
    WebSocket,
    ContentFooter
  }
}
</script>
