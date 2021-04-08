<template>
  <div>
    <h3>
      Translate
    </h3>
    <div v-if="loaded">
      <button v-on:click="ret">Return</button>
      <div v-if="conversation">
        <div align="right" style="color: rgb(120, 80, 0);">{{selected}}</div>
        <b-list-group>
          <div v-for="message in conversation" :key="message.index">
            <b-list-group-item>
              <h5 align="left">{{ message.chatStyle }}</h5>
              <div align="left" style="font-weight: 600; color: gray;">Original:</div>
              <p align="left" style="font-size: 150%;">{{ message.text }}</p>
              <div align="left" style="font-weight: 600; color: gray;">Translation:</div>
              <p align="left" style="font-size: 150%;">{{ message.translation }}</p>
            </b-list-group-item>
          </div>
        </b-list-group>
        <hr/>
      </div>

      <div v-else>
        Please chat.
      </div>
    </div>

    <div v-else>

      Select a Language to translate conversation to.
      <br>
      <select v-model="selected">
        <option>Korean</option>
        <option>Japanese</option>
        <option>Chinese</option>
        <option>Hindi</option>
        <option>French</option>
        <option>Punjabi</option>
        <option>Russian</option>
        <option>Spanish</option>
        <option>Urdu</option>
        <option>Italian</option>
      </select>
      <button v-on:click="start">Translate</button>

    </div>


  </div>
</template>

<script>
import translate from "translate";
export default {
  data() {
    return {
      conversation: undefined,
      iterator: 0,
      loaded: false,
      selected: ''
    };
  },
  created() {
    if (this.$store.state.conversation){
      this.conversation = this.$store.state.conversation;
    }
  },
  methods: {
    start: function () {
      if(this.selected != null || this.selected != '' || this.selected != ' ')
        this.Translate();
    },
    ret: function () {
      this.loaded = false;
      this.iterator = 0;
    },
    Translate() {
      translate.engine = "libre";
      if (this.conversation != null)
      {
        for (var i = 0; i < this.conversation.length; i++)
        {
          translate(this.conversation[i].text, this.selected).then(value => this.SetTranslation(value));
        }
      }
    },
    SetTranslation(value) {
      this.conversation[this.iterator].translation = value;
      this.iterator++;
      if(this.iterator >= this.conversation.length)
      {
        this.loaded = true;
      }
    }
  }
};
</script>

<style>
h5 {
  color: rgb(116, 0, 170);
  font-weight: 600;
}
tr:nth-child(even) {
  background-color: #f7f7f7;
}
tr:hover {
  background-color: rgb(234, 225, 238);
}
</style>