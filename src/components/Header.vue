<template>
  <div style="margin-bottom: 200px">
    <div
        class="white--text headline"
        style="width: 400px; float:left; display: inline-block; cursor: pointer"
        @click.prevent="onClick"
    >Halfsms Message Converter
    </div>

    <div style="display: inline-block; float: right">
      <div class="text-xs-center">
        <v-btn round color="white" href="https://github.com/jwooss/vue-message-converter" target="_blank">Github</v-btn>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'Header',
    methods: {
      onClick() {
        location.href = "/"
      }
    }
  }
</script>

<style scoped>

</style>
