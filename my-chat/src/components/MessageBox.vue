<template>
  <v-container bg fill-height grid-list-md text-xs-center>
    <v-row :justify="chatJustify">
      <v-col cols="10">
        <div>
          <v-card
            class="align-center justify-center"
            :variant="'outlined'"
            :color="chatColor"
          >
            <v-card-text>
              <v-list-item-title v-text="user"></v-list-item-title>
              <v-list-item-text v-text="text"></v-list-item-text>
            </v-card-text>
          </v-card>
        </div>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: "MessageBox",
  props: {
    user: String,
    text: String,
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
  computed: {
    chatColor() {
      return this.user == "Chatbot" ? "teal-darken-4" : "secondary";
    },
    chatJustify() {
      return this.user == "Chatbot" ? "start" : "end";
    },
  },
};
</script>
