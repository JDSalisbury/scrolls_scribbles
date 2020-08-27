<template>
  <v-card class="menu">
    <v-toolbar flat color="primary" dark>
      <!-- <v-toolbar-title>User Profile</v-toolbar-title> -->
      <v-btn class="mr-4" @click="saveNotes">Save</v-btn>
      <v-file-input
        show-size
        label="File input"
        @change="selectFile"
      ></v-file-input>
    </v-toolbar>
    <v-tabs vertical>
      <div v-for="item in tags" :key="item.id">
        <v-tab>
          {{ item.name }}
        </v-tab>
      </div>

      <v-tab-item v-for="item in groupList" :key="item.id">
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
                Name: {{ listItem.name }} {{ listItem.created_at }}
                <p>Location: {{ listItem.location }}</p>
                Info: {{ listItem.info }}
                <div class="extra content">
                  <span @click="showForm(listItem)">
                    <v-icon dark>mdi-pencil</v-icon>
                  </span>
                  <span @click="deleteItem(listItem, item)">
                    <v-icon dark>mdi-delete</v-icon>
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
    groupList: [
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
    deleteItem(item, groupList_item) {
      let newList = groupList_item.list.filter(
        (li) => li.created_at != item.created_at
      );

      this.groupList.map((groupList) => {
        if (groupList.group == groupList_item.group) {
          groupList.list = newList;
        }
      });
    },
    addItem(groupList) {
      groupList.adding = true;
    },
    addToList(groupList) {
      let item_clone = Object.assign({}, this.itemToAdd);
      item_clone.created_at = Date();
      groupList.list.push(item_clone);
      this.finsishAdding(groupList);
    },
    finsishAdding(groupList) {
      groupList.adding = false;
      this.itemToAdd.name = '';
      this.itemToAdd.location = '';
      this.itemToAdd.info = '';
    },
    saveNotes() {
      var FileSaver = require('file-saver');

      var json = JSON.stringify(this.$data);
      var blob = new Blob([json], {
        type: 'text/plain;charset=utf-8'
      });
      FileSaver.saveAs(blob, 'scrolls_and_scribbles.txt');
    },
    selectFile(file) {
      function parseFile(file) {
        return new Promise((resolve, reject) => {
          let content;
          const reader = new FileReader();

          reader.onloadend = function(e) {
            content = JSON.parse(e.target.result);
            resolve(content);
          };
          reader.onerror = function(e) {
            reject(e);
          };
          reader.readAsText(file);
        });
      }
      parseFile(file).then(
        (result) => (this.groupList = result.groupList),
        (error) => alert(error)
      );
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
