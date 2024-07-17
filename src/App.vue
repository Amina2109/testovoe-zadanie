<template>
  <v-app>
    <v-container>
      <v-row>
        <v-col cols="12">
          <TextTags :tags="tags" alignment="left" />
        </v-col>
        <v-col cols="12">
          <TextTags :tags="tags" alignment="justify" />
        </v-col>
        <v-col cols="6">
          <TextTags :tags="tags" alignment="left" />
        </v-col>
        <v-col cols="6">
          <TextTags :tags="tags" alignment="justify" />
        </v-col>
      </v-row>
    </v-container>
  </v-app>
</template>

<script>
import TextTags from './components/TextTags.vue';

export default {
  name: 'App',
  components: {
    TextTags
  },
  data() {
    return {
      tags: [
        { text: 'текст1' },
        { text: 'текст2', icon: 'mdi-star' },
        { text: 'текст3' },
        { text: 'текст4', icon: 'mdi-heart' },
        { text: 'текст5' }
      ]
    };
  }
};
</script>

<style>
@import '~vuetify/dist/vuetify.min.css';
</style>
