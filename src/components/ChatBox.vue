<template>
  <v-card class="mx-auto text-center w-75" :color="secondary">
    <v-toolbar color="cyan-lighten-1">
      <v-btn icon="mdi-menu" variant="text"></v-btn>

      <v-toolbar-title>Chatbot</v-toolbar-title>

      <v-spacer></v-spacer>

      <v-btn icon="mdi-magnify" variant="text"></v-btn>
    </v-toolbar>

    <v-list item-props class="text-left">
      <v-list-item
        v-for="(chat, i) in chats"
        :key="i"
        color="primary"
        rounded="xl"
      >
        <MessageBox :user="chat.user" :text="chat.text"></MessageBox>
      </v-list-item>
    </v-list>

    <v-responsive class="mx-auto" max-width="700"
      ><v-text-field
        background-color="grey darken-3"
        flat
        solo
        type="text"
        v-model="query"
        id="query"
        name="query"
      >
        <template #append>
          <v-btn
            :loading="loading"
            class="flex-grow-1"
            height="48"
            variant="tonal"
            color="secondary"
            @click="askQuestion($event)"
          >
            Submit
          </v-btn>
        </template>
      </v-text-field>
    </v-responsive>
  </v-card>
</template>

<script>
import { createClient } from "@supabase/supabase-js";
import MessageBox from "./MessageBox.vue";
const supabaseClient = createClient(
  process.env.VUE_APP_SUPA_PROJECT_URL,
  process.env.VUE_APP_SUPA_API_KEY,
  {
    global: {
      headers: { Authorization: `Bearer ${process.env.VUE_APP_SUPA_API_KEY}` },
    },
  }
);

export default {
  name: "ChatBox",
  components: {
    MessageBox,
  },
  computed: {
    height() {
      switch (this.$vuetify.breakpoint.name) {
        case "xs":
          return 220;
        case "sm":
          return 400;
        case "lg":
          return 600;
        case "xl":
          return 800;
        default:
          return 500;
      }
    },
  },
  data: () => ({
    loading: false,
    chats: [
      {
        user: "Chatbot",
        text: "Hello, How can I help",
      },
    ],
  }),
  methods: {
    async askQuestion() {
      this.loading = true;
      this.chats.push({ user: "user", text: this.query });
      let { data, error } = await supabaseClient.functions.invoke("ask-query", {
        body: JSON.stringify({ query: this.query }),
      });

      if (error == null) this.chats.push({ user: "Chatbot", text: data.text });
      this.loading = false;
    },
  },
};
</script>
