<template>
  <div class="container py-5">
    <div class="d-flex justify-content-center mb-3">
      <img src="/img/nuxt.svg" class="img img-responsive mr-5" style="max-width:30%;" />
      <img src="/img/openai.svg" class="img img-responsive" style="max-width:30%;" />
    </div>
    <label>Ask OpenAI a question</label>
    <query-input v-model="queryInput" @isLoading="isLoading = $event; $event == true ? content = AIImage = null : outputKey++" @contentReceived="content = $event"  @imageReceived="AIImage = $event"/>
    <div class="card mt-5" :key="'output-card-' + outputKey">
      <div class="card-title border-bottom px-3 py-2">
        Please input query and generate ChatGPT output
      </div>
      <div class="card-body" v-if="isLoading == false && content" v-html="content">
      </div>
      <div class="card-body" v-else-if="isLoading == false && AIImage"><img :src="AIImage" class="img img-fluid" /></div>
      <div class="card-body p-5 text-center" v-else-if="isLoading == false">Output will appear here</div>
      <div class="card-body p-5 text-center" v-else-if="isLoading">Loading...</div>
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
      outputKey:1,
      isLoading: false,
      content: "",
      AIImage: null,
      queryInput:"",
    };
  },
};
</script>
