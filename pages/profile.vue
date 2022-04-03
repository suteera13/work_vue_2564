<template>
  <div>
    <v-alert>
      <input id="file" ref="file" type="file" depressed @change="handleFileUpload">
      <br>
      <img :src="file" width="100%">
    </v-alert>
    <v-btn depressed color="error">
      save
    </v-btn>
    <v-btn depressed>
      cancel
    </v-btn>
  </div>
</template>
<script>
import axios from 'axios'
export default {
  data () {
    return {
      file: ''
    }
  },
  methods: {
    async submitFile () {
      const formData = new FormData()
      formData.append('file', this.$refs.file.files[0])
      const res = await axios.post('http://localhost:7000/imgup', formData, {
        headers: {
          'Content-Type': 'multipart/form-data'
        }
      })
      console.log('ress=', res)
    },
    handleFileUpload (e) {
      // this.file = this.$refs.file.files[0]
      const image = e.target.files[0]
      const reader = new FileReader()
      reader.readAsDataURL(image)
      reader.onload = (e) => {
        this.file = e.target.result
        console.log('file', this.file)
      }
      console.log('file', this.file)
    }
  }
}
</script>
