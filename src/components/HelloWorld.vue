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
              <div class="content" v-show="!listItem.editing">
                Name: {{ listItem.name }}
                <p>Location: {{ listItem.location }}</p>
                Info: {{ listItem.info }}
                <div class="extra content">
                  <span v-on:click="showForm(listItem)">
                    <v-icon dark>mdi-pencil</v-icon>
                  </span>
                </div>
              </div>

              <div class="content" v-show="listItem.editing">
                <div class="ui form">
                  <div class="field">
                    <label>Name: </label>
                    <input type="text" v-model="listItem.name" />
                  </div>
                  <div class="field">
                    <label>Location: </label>
                    <input type="text" v-model="listItem.location" />
                  </div>
                  <br />
                  <div class="field">
                    <label>Info: </label>
                    <input type="text" v-model="listItem.info" />
                  </div>
                  <br />

                  <div class="ui two button attached buttons">
                    <button
                      class="ui basic blue button"
                      v-on:click="hideForm(listItem)"
                    >
                      Close X
                    </button>
                  </div>
                </div>
              </div>
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
    isEditing: false,
    tags: [
      { id: 1, name: 'Contacts' },
      { id: 2, name: 'Locations' },
      { id: 3, name: 'Loot' }
    ],
    desc: [
      {
        group: 'Contacts',
        list: [
          {
            name: 'Jeff',
            editing: false,
            location: 'Earth',
            info: 'Does Programming'
          },
          {
            name: 'Jeff',
            editing: false,
            location: 'Earth',
            info: 'Does Programming'
          },
          {
            name: 'Jeff',
            editing: false,
            location: 'Earth',
            info: 'Does Programming'
          },
          {
            name: 'Jeff',
            editing: false,
            location: 'Earth',
            info: 'Does Programming'
          }
        ]
      },
      {
        group: 'Locations',
        list: [
          {
            name: 'Jeff',
            editing: false,
            location: 'Earth',
            info: 'Does Programming'
          },
          {
            name: 'Jeff',
            editing: false,
            location: 'Earth',
            info: 'Does Programming'
          },
          {
            name: 'Jeff',
            editing: false,
            location: 'Earth',
            info: 'Does Programming'
          },
          {
            name: 'Jeff',
            editing: false,
            location: 'Earth',
            info: 'Does Programming'
          }
        ]
      },
      {
        group: 'Loot',
        list: [
          {
            name: 'Knife',
            editing: false,
            location: 'Earth',
            info: 'Does cutting'
          },
          {
            name: 'Gold idol',
            editing: false,
            location: 'Earth',
            info: 'Does Blinging'
          },
          {
            name: 'Posin ivy',
            editing: false,
            location: 'Earth',
            info: 'Does itching'
          },
          {
            name: '30 zeni',
            editing: false,
            location: 'Earth',
            info: 'Does buying'
          }
        ]
      }
    ]
  }),
  methods: {
    showForm(item) {
      item.editing = true;
    },
    hideForm(item) {
      item.editing = false;
    }
  }
};
</script>
<style scoped>
.menu {
  padding: 30px;
}
input {
  color: white;
}
</style>
