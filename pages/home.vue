<template>
  <div class="rounded-xl white text-center">
    <div class="txt01">
      <p>Page sign up</p>
      <h3>Hello {{ name }}</h3>
      <h3>Hello {{ upperName }}</h3>
      <input type="text" :value="name">
      <input v-model="name" type="text">
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      name: 'Suteera'
    }
  },
  computed: {
    upperName () {
      return this.name.toUpperCase()
    }
  }
}
</script>

<style>
    .txt01 {
        color: rgb(21, 79, 240);
    }
</style>
