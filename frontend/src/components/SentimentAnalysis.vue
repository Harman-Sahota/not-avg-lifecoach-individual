<template>
  <div>
    <h3>Sentiment Analysis</h3>
    <br />
    <BotReplyAccordian
      v-if="repliesAndSentimentAnalyses"
      :repliesAndSentimentAnalyses="repliesAndSentimentAnalyses"
      :conversation="conversation"
    />
    <div v-else style="color:white">
      Please go to chat
    </div>
  </div>
</template>

<script>
import BotReplyAccordian from "@/components/BotReplyAccordian.vue";

export default {
  components: {
    BotReplyAccordian
  },
  data() {
    return {
      repliesAndSentimentAnalyses: undefined,
      conversation: []
    };
  },
  created() {
    if (this.$store.state.conversation && this.$store.state.allConvoData) {
      this.repliesAndSentimentAnalyses = this.$store.state.allConvoData.activities;
      this.conversation = this.$store.state.conversation;
      this.conversation.shift();
    }
  }
};
</script>

<style>
.btn-info{
background-color:#DA7B93;
 }
 .btn-info:hover{
background-color:#DA7B93;
 }
</style>
