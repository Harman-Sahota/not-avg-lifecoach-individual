<template>
  <div>
    <b-navbar toggleable="lg" type="dark" variant="info">
      <b-navbar-brand fluid href="#"
        >Not Your Average Life Coach</b-navbar-brand
      >
      <button @click="goToChatRoute" class="button-link">Chat</button>
    </b-navbar>
    <b-container fixed fluid>
      <br />
      <h1 class="text-light">Chat Analysis</h1>
      <br />
      <SentimentAnalysis v-show="showComponent.sentimentAnalysis" />
      <NamedEntityRecognition v-show="showComponent.namedEntityRecognition" />
      <PartsOfSpeech v-show="showComponent.partOfSpeech" />
      <SynonymRecognition v-show="showComponent.synonymRecognition" />
      <Wikipedia v-show="showComponent.wikipedia" />

      <b-row class="py-5 my-5"
        ><hr />
        <hr />
      </b-row>
    </b-container>
    <b-navbar
      toggleable="lg"
      type="dark"
      fixed="bottom"
      variant="info"
      class="center"
     
    >
      <b-navbar-nav class="mx-auto">
        <b-nav-item @click="showPartOfSpeech"
          >Part of Speech Tagging</b-nav-item
        >
        <b-nav-item @click="showSentimentAnalysis"
          >Sentiment Analysis</b-nav-item
        >
        <b-nav-item @click="showNamedEntityRecognition"
          >Named Entity Recognition</b-nav-item
        >
        <b-nav-item @click="showSynonymRecognition"
          >Synonym Recognition</b-nav-item
        >
        <b-nav-item @click="showWikipedia"
          >Wikipedia</b-nav-item
        >
      
      </b-navbar-nav>
    </b-navbar>
  </div>
</template>

<script>
import SentimentAnalysis from "@/components/SentimentAnalysis.vue";
import NamedEntityRecognition from "../components/NamedEntityRecognition.vue";
import PartsOfSpeech from "../components/PartsOfSpeech.vue";
import SynonymRecognition from "../components/SynonymRecognition.vue";
import Wikipedia from "../components/Wikipedia.vue";

export default {
  data() {
    return {
      showComponent: undefined
    };
  },
  created() {
    this.showComponent = this.$store.state.currentChatAnalysisComponent;
  },
  components: {
    SentimentAnalysis,
    PartsOfSpeech,
    NamedEntityRecognition,
    SynonymRecognition,
    Wikipedia
  },
  methods: {
    goToChatRoute() {
      this.$router.push("/");
    },

     showWikipedia() {
      this.$store.commit("setConversation", this.conversation);
      this.$store.commit("setCurrentChatAnalysisComponentBools", {
        sentimentAnalysis: false,
        partOfSpeech: false,
        namedEntityRecognition: false,
        synonymRecognition: false,
        wikipedia: true,
        translate: false
      });
      this.updateComponent();
    },
    showSentimentAnalysis() {
      this.$store.commit("setConversation", this.conversation);
      this.$store.commit("setCurrentChatAnalysisComponentBools", {
        sentimentAnalysis: true,
        partOfSpeech: false,
        namedEntityRecognition: false,
        synonymRecognition: false
      });
      this.updateComponent();
    },
    showPartOfSpeech() {
      this.$store.commit("setConversation", this.conversation);
      this.$store.commit("setCurrentChatAnalysisComponentBools", {
        sentimentAnalysis: false,
        partOfSpeech: true,
        namedEntityRecognition: false,
        synonymRecognition: false
      });
      this.updateComponent();
    },

    showSynonymRecognition() {
      this.$store.commit("setConversation", this.conversation);
      this.$store.commit("setCurrentChatAnalysisComponentBools", {
        sentimentAnalysis: false,
        partOfSpeech: false,
        namedEntityRecognition: false,
        synonymRecognition: true
      });
      this.updateComponent();
    },

    showNamedEntityRecognition() {
      this.$store.commit("setConversation", this.conversation);
      this.$store.commit("setCurrentChatAnalysisComponentBools", {
        sentimentAnalysis: false,
        partOfSpeech: false,
        namedEntityRecognition: true,
        synonymRecognition: false
      });
      this.updateComponent();
    },
    updateComponent() {
      this.showComponent = this.$store.state.currentChatAnalysisComponent;
    }
  }
};
</script>

<style>
h3{
color:white;
}
.list-group-item{
background-color:#2e151b;
color:white;
}
h5{
color:#DA7B93;
}
th{
background-color:pink !important;
}
tr{
background-color:white !important;
border-bottom:1px solid #2e151b;
}
.navbar{
background-color:#376e64 !important;
}
</style>
