<template>
  <v-layout
    row>

    <v-flex xs12>
      <v-select v-model="selectedPerson" :items="people" item-text="name" @change="update"/>
    </v-flex>

    <Reactions :reactions="reactions" title="Reacted To"/>

    <Reactions :reactions="reacted" title="Got Reacted"/>

    <Reactions :reactions="reactionsPerReacts" title="Reacted To/Got Reacted"/>

    <Reactions :reactions="reactedPerReactions" title="Got Reacted/Reacted To"/>


  </v-layout>

</template>

<script>
  import axios from "~/plugins/axios";
  import ReactionCount from "../components/ReactionCount";
  import Reactions from "../components/Reactions";

  export default {
    name: "reactions",
    components: {ReactionCount, Reactions},
    data: function () {
      return {
        selectedPerson: {},
        people: [],
        reactions: [],
        reacted: []
      }

    },
    computed: {
      reactionsPerReacts: function () {
        let perReacts = [];
        for (const reaction of this.reactions) {
          for (const reacted of this.reacted) {
            console.log(reaction);
            console.log(reacted);
            if(reaction.reaction === reacted.reaction){
                perReacts.push({
                  reaction: reaction.reaction,
                  count : Math.round((reaction.count / reacted.count) * 100000) / 100000
                });
            }
          }
        }
        return perReacts;
      },
      reactedPerReactions: function () {
        let perReacts = [];
        for (const reaction of this.reactions) {
          for (const reacted of this.reacted) {
            console.log(reaction);
            console.log(reacted);
            if(reaction.reaction === reacted.reaction){
              perReacts.push({
                reaction: reaction.reaction,
                count : Math.round(( reacted.count / reaction.count) * 100000) / 100000
              });
            }
          }
        }
        return perReacts;
      }
    },
    async mounted() {
      let response = await axios.get("/people");
      this.people = response.data;
    },
    methods: {
      'update': async function () {
        let reacts = await axios.get("/reactions/actor", {params: {sender: this.selectedPerson}});
        let reacted = await axios.get("/reactions/sender", {params: {sender: this.selectedPerson}});
        this.reactions = reacts.data;
        this.reacted = reacted.data;

      }
    }


  }
</script>

<style scoped>

</style>
