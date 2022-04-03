<template>
  <div>
    <div class="text-center">
      <v-dialog
        v-model="dialog"
        width="500"
      >
        <v-card>
          <v-card-title class="text-h5 grey lighten-2">
            Status Login
          </v-card-title>

          <v-card-text>
            SUCCESS  LOGIN
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
            Status Login
          </v-card-title>

          <v-card-text>
            ERROR  LOGIN
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

    <v-row justify="center" align="center">
      <v-col cols="12" sm="8" md="6">
        <div
          class="pa-7 white rounded-circle d-inline-block"
        >
          <v-icon large color="blue">
            mdi-cloud-upload
          </v-icon>
        </div>
      </v-col>
    </v-row>
    <h1 style="margin-top:20px;">
      KNN BANK
    </h1>
    <h3>MOBILE APP</h3>
    <v-form>
      <v-form>
        <v-text-field
          v-model="user"
          label="PLEATE ENTER YOUR USERNAME"
          placeholder="USERNAME OR EMAIL"
          filled
          rounded
          dense
        />
        <v-text-field
          v-model="passwd"
          label="PLEATE ENTER YOUR PASSWORD"
          placeholder="PASSWORD"
          filled
          rounded
          dense
        />
        <v-btn class="btn1" @click="onSave">
          <span class="txt1">LOG IN</span>
        </v-btn>
        <p>Forgot password?</p>
        <p>
          New to Bank Apps?
          Sign Up
        </p>
      </v-form>
        <!-- <v-row>
          <v-col
            cols="12"
            sm="6"
            md="4"
          >
            <v-menu transition="scroll-y-transition">
              <template #activator="{ on, attrs }">
                <v-text-field
                  label="PLEATE ENTER YOUR PASSWORD"
                  placeholder="PASSWORD"
                  filled
                  rounded
                  dense
                  v-bind="attrs"
                  v-on="on"
                />
              </template>
              <div class="box">
                <v-row>
                  <v-col class="d-flex justify-center">
                    <v-btn
                      class="mx-2"
                      fab
                      dark
                      color="primary"
                    >
                      1
                    </v-btn>
                    <v-btn
                      class="mx-2"
                      fab
                      dark
                      color="primary"
                    >
                      2
                    </v-btn>
                    <v-btn
                      class="mx-2"
                      fab
                      dark
                      color="primary"
                    >
                      3
                    </v-btn>
                  </v-col>
                </v-row>

                <v-row>
                  <v-col class="d-flex justify-center">
                    <v-btn
                      class="mx-2"
                      fab
                      dark
                      color="primary"
                    >
                      4
                    </v-btn>
                    <v-btn
                      class="mx-2"
                      fab
                      dark
                      color="primary"
                    >
                      5
                    </v-btn>
                    <v-btn
                      class="mx-2"
                      fab
                      dark
                      color="primary"
                    >
                      6
                    </v-btn>
                  </v-col>
                </v-row>
                <v-row>
                  <v-col class="d-flex justify-center">
                    <v-btn
                      class="mx-2"
                      fab
                      dark
                      color="primary"
                    >
                      7
                    </v-btn>
                    <v-btn
                      class="mx-2"
                      fab
                      dark
                      color="primary"
                    >
                      8
                    </v-btn>
                    <v-btn
                      class="mx-2"
                      fab
                      dark
                      color="primary"
                    >
                      9
                    </v-btn>
                  </v-col>
                </v-row>
                <v-row>
                  <v-col class="d-flex justify-center">
                    <v-btn
                      class="mx-2"
                      fab
                      dark
                      color="primary"
                    >
                      0
                    </v-btn>
                    <v-btn
                      class="mx-2"
                      fab
                      dark
                      color="warning"
                    >
                      <i class="fas fa-times" />
                    </v-btn>
                  </v-col>
                </v-row>
              </div>
            </v-menu>
            <v-btn class="btn1">
              <span class="txt1">LOG IN</span>
            </v-btn>
            <p>Forgot password?</p>
            <p>
              New to Bank Apps?
              Sign Up
            </p>
          </v-col>
        </v-row>
      </v-container>
    </v-form>
  </div>
</template>
            </v-menu>
          </v-col>
        </v-row> -->
      </v-container>
    </v-form>
  </div>
</template>

<script>
export default {
  data: () => ({
    user: '',
    passwd: '',
    dialog: false,
    dialog_false: false,
    fname: ''
  }),
  methods: {
    async  onSave () {
      console.log('onSave')
      const res = await fetch('http://localhost:7000/list_user?user=' +
      this.user + '&pass=' + this.passwd)
      const result = await res.json()
      try {
        console.log('data=', result.data)
        this.fname = result.data.firstname
        if (result.status > 0) {
          console.log('Login ok')
          this.dialog = true
          setInterval(() => {
            this.dialog = false
            this.$router.push('/datastd')
          }, 4000)
        }
      } catch (error) {
        console.log('Error Login')
        this.dialog_false = true
        setInterval(() => {
          this.dialog_false = false
          this.$router.push('/')
        }, 3000)
      }
    }
  }
}
</script>

<style>
.pa-7{
  margin-top: 100px;
}
h3 {
  margin-bottom: 50px;
}
.txt1{
  color:brown;
}
.btn1{
   background: linear-gradient(to bottom,#FFEB3B,#FFC107);
   margin-bottom: 20px;
}
.box{
  background-color:white;
  padding: 5px;
  border-radius: 25px;
  font-size: 18px;
}
</style>
