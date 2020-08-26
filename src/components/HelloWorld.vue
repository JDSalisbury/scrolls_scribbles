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
        <v-card>
          <v-icon @click="addItem(item)" dark>mdi-plus</v-icon>
          <v-form ref="form" v-if="item.adding">
            <v-layout row>
              <v-flex xs8>
                <v-text-field
                  v-model="itemToAdd.name"
                  label="Name"
                  required
                ></v-text-field>
              </v-flex>
              <v-flex xs8>
                <v-text-field
                  v-model="itemToAdd.location"
                  label="Note"
                  required
                ></v-text-field>
              </v-flex>
              <v-flex xs8>
                <v-text-field
                  v-model="itemToAdd.info"
                  label="Quantity"
                  required
                ></v-text-field>
              </v-flex>
            </v-layout>

            <v-btn class="mr-4" @click="addToList(item)">Submit</v-btn>
            <v-btn class="mr-4" @click="finsishAdding(item)">Close</v-btn>
          </v-form>

          <v-card-text>
            <div v-for="(listItem, index) in item.list" :key="index">
              <div class="content" v-show="!listItem.editing">
                Name: {{ listItem.name }}
                <p>Location: {{ listItem.location }}</p>
                Info: {{ listItem.info }}
                <div class="extra content">
                  <span @click="showForm(listItem)">
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
                      @click="hideForm(listItem)"
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
    adding: false,
    itemToAdd: {
      name: '',
      info: '',
      location: '',
      adding: false,
      editing: false
    },
    tags: [
      { id: 1, name: 'Contacts' },
      { id: 2, name: 'Locations' },
      { id: 3, name: 'Loot' }
    ],
    desc: [
      {
        adding: false,
        group: 'Contacts',
        list: []
      },
      {
        adding: false,
        group: 'Locations',
        list: []
      },
      {
        adding: false,
        group: 'Loot',
        list: []
      }
    ]
  }),
  methods: {
    showForm(item) {
      item.editing = true;
    },
    hideForm(item) {
      item.editing = false;
    },
    addItem(desc) {
      desc.adding = true;
    },
    addToList(desc) {
      let item_clone = Object.assign({}, this.itemToAdd);
      desc.list.push(item_clone);
      this.finsishAdding(desc);
    },
    finsishAdding(desc) {
      desc.adding = false;
      this.itemToAdd.name = '';
      this.itemToAdd.location = '';
      this.itemToAdd.info = '';
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
