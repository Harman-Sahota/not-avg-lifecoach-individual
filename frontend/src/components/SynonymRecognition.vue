<template>
  <div>
    <h3>
      Synonym Recognition
    </h3>
    <div v-if="conversation">
      <div v-html="fullText"></div>
      <hr />
    </div>
    <div v-else style="color:white">
      Please chat first before Synonym Recognition analysis is available
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      conversation: undefined,
      word: undefined,
      sentence: "",
      fullText: "",
      thesaurusText: ""
    };
  },
  created() {
    var posTagger = require("wink-pos-tagger");
    var thesaurus = require("thesaurus");
    if (this.$store.state.conversation)
      this.conversation = this.$store.state.conversation;

    var flag;
    this.flag = 0;
    try {
      for (var i = 0; i < this.conversation.length; i = i + 2) {
        var tagger = posTagger();
        this.word = tagger.tagSentence(this.conversation[i].text);
        this.fullText += '<hr/><table class = "table">';
        for (var f = 0; f < this.word.length; f++) {
          if (this.flag == 0) {
            this.fullText +=
              "<tr class = 'tr'><h3><b>" +
              this.conversation[i].text +
              "</h3></b></tr>";
            this.fullText +=
              "<tr class = 'tr'><th>Word</th><th>Analysis</th></tr>";
          }
          if (thesaurus.find(this.word[f].value).length > 0) {
            for (
              var g = 0;
              g < thesaurus.find(this.word[f].value).length;
              g++
            ) {
              if (g == thesaurus.find(this.word[f].value).length - 1) {
                this.thesaurusText += thesaurus.find(this.word[f].value)[g];
              } else {
                this.thesaurusText +=
                  thesaurus.find(this.word[f].value)[g] + ", ";
              }
            }
          } else {
            this.thesaurusText = "None";
          }

          this.fullText +=
            "<tr class = 'tr'><td>" +
            this.word[f].value +
            "</td><td>" +
            this.thesaurusText +
            "</td></tr>";
          this.flag = 1;
          this.thesaurusText = "";
        }
        this.flag = 0;
        this.fullText += "</table>";
      }
    } catch {}
  }
};
</script>

<style>
.table {
  width: 50%;
  margin: auto;
  border: none;
  overflow: scroll;
}
.tr:nth-child(odd) {
  background-color: #f0f0f0;
}
.tr:hover {
  background-color: rgb(234, 225, 238);
}
</style>
