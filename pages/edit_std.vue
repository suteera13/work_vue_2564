<template>
  <div>
    <h2>Edit Student {{ rid }} </h2>
    <v-text-field v-model="r_id" label="r_id" />
    <v-text-field v-model="fullname" label="fullname" />
    <v-text-field v-model="baccount" label="baccount" />
    <v-text-field v-model="tel" label="tel" />
    <v-text-field v-model="email" label="email" />
    <v-text-field v-model="passwd" label="passwd" />
    <v-btn class="mr-4">
      save
    </v-btn>
    <v-btn class="mr-4">
      cancel
    </v-btn>
  </div>
</template>

<script>
export default {
  data: () => ({
    r_id: '',
    fullname: '',
    baccount: '',
    tel: '',
    email: '',
    passwd: ''
  }),
  async created () {
    console.log('rid=', this.$route.query.r_id)
    this.rid = this.$route.query.r_id
    const res = await fetch('http://localhost:7000/list?rid=' + this.rid)
    const data = await res.json()
    console.log(data.datas)
    this.r_id = data.datas.r_id
    this.fullname = data.datas.fullname
    this.baccount = data.datas.baccount
    this.tel = data.datas.tel
    this.email = data.datas.email
    this.passwd = data.datas.passwd
  }
}
</script>
