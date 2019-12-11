<template>
  <v-card class="reaction">
    <v-layout>
      <v-flex xs6>
        <v-card-text>
          {{ reaction }}
        </v-card-text>
      </v-flex>
      <v-flex xs6>
        <v-card-text xs6>
          {{ count }}
        </v-card-text>
      </v-flex>
    </v-layout>
  </v-card>
</template>

<script>
  export default {
    name: "ReactionCount",
    props: ['reaction', 'count']
  }
</script>

<style scoped>
  .reaction {
    margin: 20px !important;
  }

</style>
