<template>
  <div class="home">
    <NavBar/>
    <WebSocket/>
  </div>
</template>

<script>
// @ is an alias to /src
import NavBar from '@/components/NavBar.vue'
import WebSocket from '@/components/WebSocket.vue'

export default {
  name: 'Home',
  components: {
    NavBar,
    WebSocket
  }
}
</script>
