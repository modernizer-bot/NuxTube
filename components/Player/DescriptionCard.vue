<template>
  <v-card class="mt-2 pa-1" v-if="descriptions">
    <v-col cols="12">
      <span v-linkified class="" style="white-space: pre-wrap;" v-html="descriptions"></span>
    </v-col>
  </v-card>
</template>
<script>
  export default {
    props: {
      descriptions: ''
    },
    data() {
      return {

      }
    }
  }
</script>
