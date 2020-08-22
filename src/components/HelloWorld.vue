<template>
  <v-card class="menu">
    <v-toolbar flat color="primary" dark>
      <!-- <v-toolbar-title>User Profile</v-toolbar-title> -->
    </v-toolbar>
    <v-tabs vertical>
      <div v-for="item in tags" :key="item.id">
        <v-tab>
          {{ item.name }}
        </v-tab>
      </div>

      <v-tab-item v-for="item in desc" :key="item.id">
        <v-card flat>
          <v-card-text>
            <div v-for="(listItem, index) in item.list" :key="index">
              Name: {{ listItem.name }}
              <p>Location: {{ listItem.location }}</p>
              <p>Info: {{ listItem.info }}</p>
              <hr />
            </div>
          </v-card-text>
        </v-card>
      </v-tab-item>
    </v-tabs>
  </v-card>
</template>

<script>
export default {
  name: 'Menu',

  data: () => ({
    tags: [
      { id: 1, name: 'Contacts' },
      { id: 2, name: 'Locations' },
      { id: 3, name: 'Loot' }
    ],
    desc: [
      {
        group: 'Contacts',
        list: [
          { name: 'Jeff', location: 'Earth', info: 'Does Programming' },
          { name: 'Jeff', location: 'Earth', info: 'Does Programming' },
          { name: 'Jeff', location: 'Earth', info: 'Does Programming' },
          { name: 'Jeff', location: 'Earth', info: 'Does Programming' }
        ]
      },
      { group: 'Locations', list: 'Something Something Darkside' },
      {
        group: 'Loot',
        list:
          'Cras sagittis. Phasellus nec sem in justo pellentesque facilisis.  Proin sapien ipsum, porta a, auctor quis, euismod ut, mi. Donec quam felis, ultricies nec, pellentesque eu, pretium quis, sem. Nam at tortor in tellus interdum sagittis.'
      }
    ]
  })
};
</script>
<style scoped>
.menu {
  padding: 30px;
}
</style>
