<template>
  <div>
    <div class="text-center">
      <v-dialog
        v-model="dialog"
        width="500"
      >
        <v-card>
          <v-card-title class="text-h5 grey lighten-2">
            Status Sign Up
          </v-card-title>

          <v-card-text>
            SUCCESS Sign Up
          </v-card-text>

          <v-divider />

          <v-card-actions>
            <v-spacer />
            <v-btn
              color="primary"
              text
              @click="dialog = false"
            >
              I accept
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </div>
    <div class="text-center">
      <v-dialog
        v-model="dialog_false"
        width="500"
      >
        <v-card>
          <v-card-title class="text-h5 grey lighten-2">
            Status Sign Up
          </v-card-title>

          <v-card-text>
            ERROR Sign Up
          </v-card-text>

          <v-divider />

          <v-card-actions>
            <v-spacer />
            <v-btn
              color="primary"
              text
              @click="dialog_false = false"
            >
              I accept
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </div>

    <div class="card1">
      <p>Sign up your bank account</p>
      <div class="card2">
        <form>
          <v-text-field
            v-model="username"
            label="Your Name"
            outlined
            dense
            light
          />
          <v-text-field
            v-model="baccount"
            label="Bank Account"
            outlined
            dense
            light
          />
          <v-text-field
            v-model="tel"
            label="Mobile phone number"
            outlined
            dense
            light
          />
          <v-text-field
            v-model="email"
            label="Email"
            outlined
            dense
            light
          />
          <v-text-field
            v-model="password"
            label="Password"
            outlined
            dense
            light
            append-icon="mdi-eye"
          />
          <p class="text-left" style="margin: -10px 20px 20px;">
            Use 6 characters with a mix of numbers and letters
          </p>
          <v-checkbox
            v-model="chk"
            label="By signing up, you agree to Bank's Term of Use & Privacy Policy."
            light
          />
          <v-row>
            <v-btn class="ma-2 btn2" color="yellow accent-4" elevation="0" @click="Save">
              <span class="txt1">Sign up</span>
            </v-btn>
            <p class="txt2">
              or
            </p>
            <v-btn class="ma-2 btn2" outlined color="indigo" onclick="Clear">
              Cancel
            </v-btn>
          </v-row>
        </form>
      </div>
      <p class="txt3">
        Already signed up? Log in
      </p>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data: () => ({
    username: '',
    baccount: '',
    tel: '',
    email: '',
    password: '',
    chk: '',
    dialog: false,
    dialog_false: false
  }),
  methods: {
    clr () {
      this.username = ''
      this.bacount = ''
      this.mobile = ''
      this.email = ''
      this.passwd = ''
    },
    async Save () {
      const std = {
        username: this.username,
        baccount: this.baccount,
        tel: this.tel,
        email: this.email,
        password: this.password,
        chk: this.chk
      }
      console.log('user:', std)
      const res = await axios.post('http://localhost:7000/Save', std)
      console.log(res.data)
      try {
        if (res.data.status > 0) {
          console.log('SUCCESS UP')
          this.dialog = true
          this.clr()
          setInterval(() => {
            this.dialog = false
            this.$router.push('/signup')
          }, 3000)
        } else {
          this.dialog_false = true
          this.clr()
          setInterval(() => {
            this.dialog_false = false
            this.$router.push('/signup')
          }, 3000)
        }
      } catch (error) {
        console.log('Error')
      }
    }
  }
}
</script>

<style>
  .card1{
    background-color: #fff;
    padding: 30px;
    border-radius: 20px;
    color: #5842ff;
    font-size: 12px;
  }
  .card2{
    padding: 10px;
  }
  .txt1{
    color: brown;
  }
  .txt2{
    font-size: 18px;
    color: #424242;
    margin: 10px 10px 10px;
  }
  .txt3{
    font-size: 14px;
    color: #424242;
    margin-top: 10px;
  }
  .btn2{
    width: 104px;
  }
</style>
