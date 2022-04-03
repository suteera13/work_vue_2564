<template>
  <v-app id="" class="lime lighten-1">
    <v-card class="e4">
      <v-card-text>
        <v-container fluid>
          <v-row>
            <v-col cols="12">
              <v-slider
                v-model="temp"
                :max="255"
                :min="18"
                label="อุณหภูมิ"
                class="align-center"
                @input="sliderInput"
              >
                <template #append>
                  <v-text-field
                    v-model="temp"
                    class="mt-0 pt-0"
                    type="number"
                    style="width: 60px"
                  />
                </template>
              </v-slider>
            </v-col>

            <v-col cols="12">
              <v-slider
                v-model="humi"
                :max="255"
                label="ความชื่น"
                class="align-center"
              >
                <template #append>
                  <v-text-field
                    v-model="humi"
                    class="mt-0 pt-0"
                    type="number"
                    style="width: 60px"
                  />
                </template>
              </v-slider>
            </v-col>
          </v-row>
        </v-container>
      </v-card-text>
    </v-card>
    <v-card
      light
      class="mx-auto"
      max-width="390"
    >
      <br>
      <div class="light">
        <h3>
          &nbsp;&nbsp;&nbsp;
          <v-icon
            x-large
            color="white"
            rounded
            class="icon"
          >
            mdi-thermometer-lines
          </v-icon>อุณหภูมิ {{ temp }}°C
        </h3>
      </div>
      <div class="light">
        <h3>
        &nbsp;&nbsp;
          <v-icon
            x-large
            color="white"
            rounded
            class="icon"
          >
            far fa-lightbulb
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
            mdi-air-filter
          </v-icon>แอร์ &nbsp;&nbsp;&nbsp; &nbsp; &nbsp;&nbsp;&nbsp;

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
            mdi-door-closed
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

      <!-- light4 -->
    </v-card>
  </v-app>
</template>
<script>
const mqtt = require('mqtt')
export default {
  data: () => ({
    sw01: '',
    humi: 66,
    temp: 24
  }),
  computed: {
    send_temp () {
      return console.log('temp_send=', this.temp)
    }
  },

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
        this.client.publish('suteera/sw01', '20')
      } else {
        this.client.publish('suteera/sw01', '0')
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
    sliderInput (e) {
      this.temp = e
      this.tmp = JSON.stringify(this.temp)
      this.client.publish('suteera/sw01', this.tmp)
    },
    openAllLight () {
      console.log('openAllLight')
      this.client.publish('suteera/sw01', '20')
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
      this.client.publish('suteera/sw01', '20')
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
    text-align: left;
}
.icon{
  background: rgb(236, 155, 62);
  border-radius: 100px;
  margin-right: 20px;
  width: 50px;
  height: 50px;
}
</style>
