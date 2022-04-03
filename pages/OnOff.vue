<template>
  <v-app id="" class="lime lighten-1">
    <v-card
      light
      class="mx-auto"
      max-width="390"
    >
      <div class="light">
        <h3>
        &nbsp;&nbsp;
          <v-icon
            x-large
            color="white"
            rounded
            class="icon"
          >
            mdi-thermometer
          </v-icon>อุณหภูมิ &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; {{ sw01 }}
        </h3>
      </div>

      <!-- อุณหภูมิ -->
      <div class="light">
        <h3>
        &nbsp;&nbsp;
          <v-icon
            x-large
            color="white"
            rounded
            class="icon"
          >
            mdi-lightbulb
          </v-icon>ทุกปลั๊ก &nbsp;&nbsp;&nbsp;

          <v-btn
            class="ma-2"
            color="#9ACD32"
            height="50px"
            width="50px"
            dark
            @click="openAllLight()"
          >
            <v-icon large dark>
              fas fa-check
            </v-icon>
          </v-btn>

          <v-btn
            class="ma-2"
            color="#A52A2A"
            height="50px"
            width="50px"
            dark
            @click="closeAll()"
          >
            <v-icon large dark>
              fas fa-times
            </v-icon>
          </v-btn>
        </h3>
      </div>

      <!-- Alllight -->
      <div class="light">
        <h3>
        &nbsp;&nbsp;
          <v-icon
            x-large
            color="white"
            rounded
            class="icon"
          >
            mdi-weather-windy
          </v-icon>แอร์ &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;

          <v-btn
            class="ma-2"
            color="#9ACD32"
            height="50px"
            width="50px"
            dark
            @click="openLight1()"
          >
            <v-icon large dark>
              fas fa-check
            </v-icon>
          </v-btn>

          <v-btn
            class="ma-2"
            color="#A52A2A"
            height="50px"
            width="50px"
            dark
            @click="closeLight1()"
          >
            <v-icon large dark>
              fas fa-times
            </v-icon>
          </v-btn>
        </h3>
      </div>
      <!-- light1 -->

      <div class="light">
        <h3>
        &nbsp;&nbsp;
          <v-icon
            x-large
            color="white"
            rounded
            class="icon"
          >
            mdi-fan
          </v-icon>พัดลม &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;

          <v-btn
            class="ma-2"
            color="#9ACD32"
            height="50px"
            width="50px"
            dark
            @click="openLight2()"
          >
            <v-icon large dark>
              fas fa-check
            </v-icon>
          </v-btn>

          <v-btn
            class="ma-2"
            color="#A52A2A"
            height="50px"
            width="50px"
            dark
            @click="closeLight2()"
          >
            <v-icon large dark>
              fas fa-times
            </v-icon>
          </v-btn>
        </h3>
      </div>
      <!-- light2 -->

      <div class="light">
        <h3>
        &nbsp;&nbsp;
          <v-icon
            x-large
            color="white"
            rounded
            class="icon"
          >
            mdi-door-sliding
          </v-icon>ประตู &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;

          <v-btn
            class="ma-2"
            color="#9ACD32"
            height="50px"
            width="50px"
            dark
            @click="openLight3()"
          >
            <v-icon large dark>
              fas fa-check
            </v-icon>
          </v-btn>

          <v-btn
            class="ma-2"
            color="#A52A2A"
            height="50px"
            width="50px"
            dark
            @click="closeLight3()"
          >
            <v-icon large dark>
              fas fa-times
            </v-icon>
          </v-btn>
        </h3>
      </div>
      <!-- light3 -->
    </v-card>
  </v-app>
</template>
<script>
const mqtt = require('mqtt')
export default {
  data: () => ({
    sw01: ''
  }),
  created () {
    // this.client = mqtt.connect('ws://mqtt.apps.ccollege.ac.th:8083/mqtt')
    this.client = mqtt.connect('ws://broker.emqx.io:8083/mqtt')
    this.client.on('connect', () => {
      console.log('เชื่อมต่อ MQTT SERVER')
      this.client.subscribe('suteera/sw60', (err) => {
        if (!err) {
          this.client.publish('room1/temp', '39')
          console.log('send public')
        }
      })
    })
    this.client.on('message', (topic, message) => {
      console.log('connect_message')
      // message is Buffer
      console.log('GOT-web:', message.toString())
      this.msg = message.toString()
      console.log('data=', this.msg.humi)
      this.sw01 = this.msg
      console.log('sw01=', this.sw01)
      if (this.msg >= 24) {
        this.client.publish('suteera/sw60', '1')
      } else {
        this.client.publish('suteera/sw60', '0')
      }
      // client.end()
    })
    setInterval(() => {
      this.client.publish('oak/msg1', '67')
    }, 5000)
  },
  beforeDestroy () {
    this.client.end()
  },
  methods: {
    openAllLight () {
      console.log('openAllLight')
      this.client.publish('suteera/sw01', '1')
      this.client.publish('suteera/sw02', '1')
      this.client.publish('suteera/sw03', '1')
    },
    closeAll () {
      console.log('closeLight1')
      this.client.publish('suteera/sw01', '0')
      this.client.publish('suteera/sw02', '0')
      this.client.publish('suteera/sw03', '0')
    },
    openLight1 () {
      console.log('On_Air')
      this.client.publish('suteera/sw01', '1')
    },
    closeLight1 () {
      console.log('Off_Air')
      this.client.publish('suteera/sw01', '0')
    },
    openLight2 () {
      console.log('On_Fan')
      this.client.publish('suteera/sw02', '1')
    },
    closeLight2 () {
      console.log('Off_Fan')
      this.client.publish('suteera/sw02', '0')
    },
    openLight3 () {
      console.log('On_Door')
      this.client.publish('suteera/sw03', '1')
    },
    closeLight3 () {
      console.log('Off_Door')
      this.client.publish('suteera/sw03', '0')
    }

  }
}
</script>

<style>
.mx-auto{
    margin-top: 2rem;
}
.light{
    margin-top: 2rem;
    margin-bottom: 2rem;
}
.icon{
  background: rgb(236, 155, 62);
  border-radius: 100px;
  margin-right: 20px;
  width: 50px;
  height: 50px;
}
</style>
