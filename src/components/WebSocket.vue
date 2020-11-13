<template>
  <div class="row">
    <div class="col-lg-6 com-sm-12">
      <b-card bg-variant="light">
        <b-form-group
          label-cols-sm="3"
          label="Location:"
          label-align-sm="right"
          label-for="nested-street"
          class="font-weight-bold"
        >
          <b-form-input v-model="address" class="com-sm-8"></b-form-input>
        </b-form-group>
        <b-form-group
          label-cols-sm="3"
        >
          <b-button @click="connect" class="mr-1" variant="success">Connect</b-button>
          <b-button @click="disconnect" class="mr-1" variant="danger">Disconnect</b-button>
        </b-form-group>

        <b-form-group
          label-cols-sm="3"
          label="Message:"
          label-align-sm="right"
          class="font-weight-bold mt-5"
        >
          <template v-if="selected === 'plain'">
            <b-form-input v-model="message"></b-form-input>
          </template>
          <template v-if="selected === 'json'">
            <vue-json-editor
              v-model="json"
              lang="kr"
              :mode="'code'"
              :expandedOnStart="true"></vue-json-editor>
          </template>

          <b-form-radio-group
            id="radio-slots"
            name="radio-inline"
            v-model="selected"
          >
            <b-form-radio value="plain">Plain text</b-form-radio>
            <b-form-radio value="json">Json</b-form-radio>
          </b-form-radio-group>
        </b-form-group>
        <b-form-group label-cols-sm="3">
          <b-button @click="sendMessage" class="mr-1" variant="info" :disabled="disabled === true">Send</b-button>
        </b-form-group>
      </b-card>
    </div>
    <div class="col-lg-6 com-sm-12">
      <b-card bg-variant="light">
        <b-card style="height: 500px;overflow-y: auto;">
          <p v-for="(log,idx) in logs" v-bind:key="idx">
            <template v-if="log.type === 'INFO'">
              {{ log.msg }}
            </template>
            <template v-if="log.type === 'ERROR'">
              <span style="color: red">{{ log.msg }}</span>
            </template>
            <template v-if="log.type === 'SENT'">
              <span style="color: green">{{ log.msg }}</span>
            </template>
            <template v-if="log.type === 'RECV'">
              <span style="color: blue">{{ log.msg }}</span>
            </template>
          </p>
        </b-card>
      </b-card>
    </div>
  </div>
</template>

<script>
import vueJsonEditor from 'vue-json-editor'
export default {
  name: 'WebSocket',
  components: {
    vueJsonEditor
  },
  data: function () {
    return {
      address: 'wss://localhost:8443',
      logs: [],
      disabled: true,
      message: '',
      selected: 'plain',
      json: {
        key: 'value'
      }
    }
  },
  methods: {
    connect () {
      if (this.socket === undefined || this.socket.readyState === 3) {
        this.socket = new WebSocket(this.address)
        this.socket.onopen = () => {
          this.logs.push({ type: 'INFO', msg: 'CONNECTED' })
          this.disabled = false
        }
        this.socket.onerror = () => {
          this.logs.push({ type: 'ERROR', msg: 'ERROR:' })
        }
        this.socket.onmessage = ({ data }) => {
          this.logs.push({ type: 'RECV', msg: 'RECV:' + data })
        }
        this.socket.onclose = (msg) => {
          this.logs.push({ type: 'ERROR', msg: 'Closed (Code: ' + msg.code + ', Message: ' + msg.reason + ')' })
        }
      }
    },
    sendMessage () {
      if (this.selected === 'plain') {
        this.logs.push({ type: 'SENT', msg: 'SENT:' + this.message })
        this.socket.send(this.message)
      } else if (this.selected === 'json') {
        this.logs.push({ type: 'SENT', msg: 'SENT:' + JSON.stringify(this.json) })
        this.socket.send(JSON.stringify(this.json))
      }
    },
    disconnect () {
      if (this.socket.readyState === 1) {
        this.socket.close()
        this.logs.push({ type: 'INFO', msg: 'DISCONNECTED' })
        this.disabled = true
      }
    }
  }
}
</script>

<style scoped>
  p{
    line-height: 1.0;
  }
</style>
