<template>
  <v-simple-table>
    <template #default>
      <thead>
        <tr>
          <th class="text-left">
            ID
          </th>
          <th class="text-left">
            fullname
          </th>
          <th class="text-left">
            baccount
          </th>
          <th class="text-left">
            img
          </th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="std in student" :key="std.id">
          <td class="text-left">
            {{ std.r_id }}
          </td>
          <td class="text-left">
            {{ std.fullname }}
          </td>
          <td class="text-left">
            {{ std.baccount }}
          </td>
          <td class="text-left">
            <img :src="std.tel" width="20%">
          </td>
          <td class="text-left">
            <v-btn @click="edit(std.r_id)">
              edit
            </v-btn>
          </td>
        </tr>
      </tbody>
    </template>
  </v-simple-table>
</template>

<script>
export default {
  layout: 'laStudent',
  data () {
    return {
      r_id: '',
      fullname: '',
      baccount: '',
      tel: '',
      email: '',
      passwd: '',
      student: [
        { r_id: '1', fullname: 'nuttagamal', email: 'g@gmail.com' },
        { r_id: '2', fullname: 'suteera', email: 'g@gmail.com' },
        { r_id: '3', fullname: 'sutaluk', email: 'g@gmail.com' },
        { r_id: '3', fullname: 'phattaranit', email: 'g@gmail.com' },
        { r_id: '3', fullname: 'wannarak', email: 'g@gmail.com' }
      ]
    }
  },
  created () {
    this.Show()
  },
  methods: {
    async Show () {
      console.log('show list')
      const res = await fetch('http://localhost:7000/lists')
      const data = await res.json()
      this.student = data.datas
      console.log(data.datas)
    },
    edit (rid) {
      console.log('rid=', rid)
      this.$router.push('edit_std?r_id=' + rid)
    }
  }
}
</script>
