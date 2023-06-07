<template>
  <div class="container py-5">
    <div class="d-flex justify-content-center mb-3">
      <img src="/img/nuxt.svg" class="img img-responsive mr-5" style="max-width:30%;" />
      <img src="/img/openai.svg" class="img img-responsive" style="max-width:30%;" />
    </div>
    <label>Ask OpenAI a question</label>
    <query-input v-model="queryInput" @isLoading="isLoading = $event" @contentReceived="content = $event" />
    <div class="card mt-5">
      <div class="card-title border-bottom px-3 py-2">
        {{ isLoading == false && !content ? "Please input query and generate ChatGPT output" : isLoading == true ? "Loading..." : "Output for \"" + queryInput + "\"" }}</div>
      <div class="card-body" v-if="isLoading == false" v-html="content">
      </div>
      </div>

    </div>
</template>

<script>
import queryInput from "@/components/queryInput";
import { Configuration, OpenAIApi } from "openai";

const configuration = new Configuration({
  apiKey: process.env.OPENAI_API_KEY,
});
const openai = new OpenAIApi(configuration);

export default {
components:{queryInput},
  data() {
    return {
      isLoading: false,
      content: "",
      queryInput:"How much is 2+2?",
    };
  },
};
</script>
