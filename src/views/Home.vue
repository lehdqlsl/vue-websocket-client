<template>
  <div class="home">
    <NavBar/>
    <div class="col-12 mt-3">
      <WebSocket/>
    </div>
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
