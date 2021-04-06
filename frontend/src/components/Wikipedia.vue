<script src="https://unpkg.com/v-tooltip@^2.0.0"></script>
<template>
 <div>
    <h2 style="color:white"><i>
      Wikipedia API
    </i></h2>
    <div v-if="loaded" >
      <div v-if="conversation" >
        <hr />
        <b-list-group >
          <b-list-group-item style="padding:2px">
            <div align="left" style="font-weight: 800; color: white;background-color:#2e151b">Wiki Results From Conversation:</div>
            <div align="left" style="color: white;background-color:#2e151b">Wikipedia results are listed as follows:</div>
              <table style="width: 100%; margin-top: 1em;" class="table table-striped table-bordered">
              <tr>
                <th style="text-align:left; background-color: #2F4454;color:#2e151b;">
                  Title
                </th>
                <th style="text-align:left; background-color: #2F4454;color:#2e151b">
                  Summary
                </th>
                </tr>
              <tr style="color: black;background-color:#2F4454" v-for="result in wikiResults" :key="result.index">
                <td align="left" style="padding: 0.25em; padding-top: 1em; padding-bottom: 1em;"><a :href='result.url' target="_blank">{{ result.title }}</a></td>
                <td align="left" style="padding: 0.25em; color: #2e151b; text-size: 50%">{{ result.extract }}</td>
              </tr>
            </table>
          </b-list-group-item>
        </b-list-group>
        <hr />
        <br>
        <div align="left" style="color: #DA7B93;">*Titles link to corresponding wikipedia page.</div>
      </div>
      <div v-else style="color:white">
        Please chat.
      </div>
    </div>
    <div v-else>
      Loading...
    </div>
  </div>

</template>
<script>
import wikipedia from "wikipedia";
export default {
  data() {
    return {
      conversation: undefined,
      wikiResults: [],
      loaded: false,
      iterator: 0
    };
  },
  created() {
    if (this.$store.state.conversation)
      this.conversation = this.$store.state.conversation;
    this.Wiki();
  },
  methods: {
    // Gets Wiki data for each word in the conversation
    Wiki() {
      if (this.conversation != null)
      {
        for (var i = 0; i < this.conversation.length; i++)
        {
          //Split input into induvidual words:
          var temp = this.conversation[i].text.replace(".", " ").replace(",", " ").replace("!", " ").replace("?", " ").split(" ");
          temp = [...new Set(temp)]; // remove duplicates
          for (var j = 0; j < temp.length; j++)
          {
            if(temp[j].length > 0)
            {
              wikipedia.page(temp[j]).then((data) => {
                data.summary().then((data) => 
                  this.SetWiki(data.title, data.extract, data.content_urls.desktop.page)) // Send wiki data to SetWiki
              })
            }
          }
        }
      }
      this.loaded = true;
    },
    // Sets Wiki data to be accessed and displayed
    SetWiki(title, extract, url) {
      this.wikiResults.push({title, extract, url});
      this.iterator++;
    }
  }
};
</script>

<style>
h5 {
  color: rgb(170, 145, 0);
  font-weight: 600;
}
tr:nth-child(even) {
  background-color: #f5f2ed;
}
tr:hover {
  background-color: rgb(234, 225, 238);
}
</style>