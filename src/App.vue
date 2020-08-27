<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <v-img
        alt="S&S word logo"
        class="shrink mr-2"
        contain
        src="./assets/s&s.png"
        transition="scale-transition"
        width="400"
      />

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
