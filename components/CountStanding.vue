<template>
    <v-card>
      <v-card-text>
        <div style="width: 10%;display: inline-block">
          {{ position }}
        </div>
        <div style="width: 70%;display: inline-block">
          {{ name }}
        </div>
        <div style="width: 19%;display: inline-block">
          {{ count }}
        </div>
      </v-card-text>
    </v-card>
</template>

<script>
    export default {
        name: "CountStanding",
        props: ['position', 'name', 'count']
    }
</script>

<style scoped>

</style>
