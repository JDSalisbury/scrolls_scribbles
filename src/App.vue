<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <div class="d-flex align-center">
        <v-img
          alt="Vuetify Logo"
          class="shrink mr-2"
          contain
          src="./assets/quill.png"
          transition="scale-transition"
          width="55"
        />
        <h1>Scrolls & Scribbles</h1>
      </div>

      <v-spacer></v-spacer>
    </v-app-bar>

    <v-main>
      <TagGroup />
    </v-main>
  </v-app>
</template>

<script>
import TagGroup from './components/TagGroup';

export default {
  name: 'App',
  components: {
    TagGroup
  }
};
</script>
<style>
body {
  background-color: #131313;
}
</style>
