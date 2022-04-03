<template>
  <div>
    <h1>Show data student</h1>
    <ul>
      <li v-for="std in student" :key="std.r_id">
        <div> {{ std.fullname }} </div>
        <div> {{ std.baccount }} </div>
        <div> {{ std.tel }} </div>
        <div> {{ std.email }} </div>
        <div> {{ std.passwd }} </div>
      </li>
    </ul>
    <v-btn color="success" class="mr-4" @click="Show">
      Show
    </v-btn>
  </div>
</template>

<script>
export default {
  data: () => ({
    fullname: '',
    baccount: '',
    tel: '',
    email: '',
    passwd: '',
    student: [
      { fullname: '001', email: 'name1' },
      { fullname: '002', email: 'name2' },
      { fullname: '003', email: 'name3' },
      { fullname: '004', email: 'name4' }
    ]
  }),
  methods: {
    async Show () {
      console.log('show list')
      const res = await fetch('http://localhost:7001/list')
      const data = await res.json()
      this.student = data.datas
      console.log(data.datas)
    }
  }
}
</script>
