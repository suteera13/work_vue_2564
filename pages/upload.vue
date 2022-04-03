<template>
  <div class="container">
    <div class="large-12 medium-12 small-12 cell">
      <label>
        File
        <input id="file" ref="file" type="file" @change="handleFileUpload">
      </label>
      <v-card height="50%" class="grey lighten-4 paddign">
        <img :src="file" width="100%">
        <v-card-actions style="font-size:100%">
          <span>
            <i class="fas fa-image fa-2x" />
          </span>
          <v-spacer />
          <span> รูปภาพ </span>
        </v-card-actions>
      </v-card>
      <button @click="submitFile()">
        Submit
      </button>
    </div>
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
