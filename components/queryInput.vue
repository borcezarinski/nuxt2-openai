<template>
  <div class="query-input-container">
    <textarea class="form-control" v-model="queryInput" @error="content = $event" @content="content=$event"></textarea>
 <div class="d-flex justify-content-between mt-3">
   <div class="mr-3 d-flex flex-column">
   <button class="btn btn-primary" :disabled="isLoading" @click="generate">Send Message</button>
 </div>
   <div><label>Max Tokens:</label><input class="form-control" style="max-width:180px;" v-model="maxTokens"></div></div>
  </div>
</template>
<script>
import { Configuration, OpenAIApi } from "openai";

const configuration = new Configuration({
  apiKey: process.env.OPENAI_API_KEY,
});
const openai = new OpenAIApi(configuration);

export default {
  name:"queryInput",
  props:{
    value:{
      type:String
    },
    max_tokens:{
      type:Number,
      default:100
    }
  },
  data() {
    return {
      isLoading: false,
      content: "",
      queryInput:this.value,
      maxTokens: this.max_tokens
    };
  },
  methods:{
    async generate() {
      if (!configuration.apiKey) {
        this.showError("OpenAI API key not configured, please follow instructions in README.md");
        return;
      }
      if (this.queryInput.trim().length === 0) {
        this.showError("Please enter a valid queryInput");
        return;
      }

      try {
        this.isLoading = true;
        this.$emit("isLoading", true);
        const completion = await openai.createCompletion({
          model: "text-davinci-003",
          prompt: this.queryInput,
          max_tokens: this.$props.max_tokens,
          temperature: 0,
          top_p: 1,
          n: 1,
          stream: false,
          logprobs: null,
          stop: ""
        });
        this.content = completion.data.choices && completion.data.choices.length ? completion.data.choices[0].text : this.showError();
        this.$emit("contentReceived", this.content)
        this.isLoading = false;
        this.$emit("isLoading", false);
      } catch (error) {
        console.error("Error with OpenAI API request:", error.message);
        this.showError("An error occurred during your request.");

      }
    },
    showError(message) {
      this.isLoading = false;
      console.log("showError");
      console.log(message);
      this.content = `<p>Error: ${message}</p>`;
      this.$emit("error", this.content)
    },
  },
};
</script>
