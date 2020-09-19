<template>
  <div class="menu">
    <v-toolbar transition="scale-transition" flat color="primary" dark>
      <v-file-input
        class="mr-12 file-input"
        show-size
        label="File input"
        @change="selectFile"
      ></v-file-input>
      <v-btn v-show="groupListCheck()" class="mr-4" @click="saveNotes"
        ><v-icon color="red" v-show="this.save" left>mdi-exclamation</v-icon>
        Save</v-btn
      >
    </v-toolbar>
    <v-tabs grow background-color="primary" center-active>
      <v-tab v-for="item in tags" :key="item.id" ripple>
        {{ item.name }}
      </v-tab>

      <v-tab-item v-for="item in groupList" :key="item.id">
        <div class="form mr-8">
          <v-btn
            class="btn-add"
            outlined
            @click="addItem(item)"
            color="success"
            fab
            x-small
            dark
          >
            <v-icon dark>mdi-plus</v-icon>
          </v-btn>
          <v-form class="form-area" ref="form" v-if="item.adding">
            <v-layout class="form-inputs">
              <v-flex xs8>
                <v-text-field
                  v-model="itemToAdd.name"
                  label="Name"
                  required
                ></v-text-field>
              </v-flex>
              <v-flex xs8>
                <v-text-field
                  v-model="itemToAdd.info"
                  label="Info"
                  required
                ></v-text-field>
              </v-flex>
              <v-flex xs8>
                <v-textarea
                  v-model="itemToAdd.details"
                  label="Details"
                  required
                ></v-textarea>
              </v-flex>
            </v-layout>

            <v-btn class="mr-4" @click="addToList(item)">Submit</v-btn>
            <v-btn class="mr-4" @click="finishAdding(item)">Close</v-btn>
          </v-form>
        </div>

        <div
          class="list-item"
          v-for="(listItem, index) in item.list"
          :key="index"
        >
          <v-card class="mx-auto" v-show="!listItem.editing">
            <v-card-text>
              <div>{{ listItem.info }}</div>
              <p class="display-1 text--primary">
                {{ listItem.name }}
              </p>
              <p>{{ listItem.created_at }}</p>
              <p class="text--primary detail-area">{{ listItem.details }}</p>
            </v-card-text>
            <v-card-actions>
              <span @click="showForm(listItem)">
                <v-icon dark>mdi-pencil</v-icon>
              </span>
              <span @click="deleteItem(listItem, item)">
                <v-icon dark>mdi-delete</v-icon>
              </span>
            </v-card-actions>
          </v-card>

          <v-card class="mx-auto" v-show="listItem.editing">
            <v-card-text>
              <div class="field">
                <label>Info: </label>
                <input type="text" v-model="listItem.info" />
              </div>
              <div class="display-1 text--primary field">
                <label>Name: </label>
                <input type="text" v-model="listItem.name" />
              </div>
              <p>{{ listItem.created_at }}</p>
              <div class="text--primary">
                <div class="field">
                  <label>Details: </label>
                  <textarea
                    :rows="lines"
                    cols="100"
                    class="text-area"
                    type="text"
                    v-model="listItem.details"
                  />
                </div>
              </div>
            </v-card-text>
            <v-card-actions>
              <v-btn class="mr-4" @click="hideForm(listItem)">Done</v-btn>
            </v-card-actions>
          </v-card>
        </div>
      </v-tab-item>
    </v-tabs>
  </div>
</template>

<script>
export default {
  name: 'Menu',

  data: () => ({
    lines: 5,
    save: false,
    adding: false,
    itemToAdd: {
      name: '',
      details: '',
      info: '',
      adding: false,
      editing: false
    },
    tags: [
      { id: 1, name: 'Persons' },
      { id: 2, name: 'Places' },
      { id: 3, name: 'Things' },
      { id: 4, name: 'Ideas' }
    ],
    groupList: [
      {
        adding: false,
        group: 'Persons',
        list: []
      },
      {
        adding: false,
        group: 'Places',
        list: []
      },
      {
        adding: false,
        group: 'Things',
        list: []
      },
      {
        adding: false,
        group: 'Ideas',
        list: []
      }
    ]
  }),
  methods: {
    shouldSave() {
      this.save = true;
    },
    showForm(item) {
      item.editing = true;
      let detailLines = item.details.split('\n');
      let total = 0;
      detailLines.forEach((detailLine) => {
        total += detailLine.length / 100;
      });
      this.lines = Math.floor(total) + 5;
    },
    hideForm(item) {
      item.editing = false;
      this.shouldSave();
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
      this.shouldSave();
    },
    addItem(groupList) {
      groupList.adding = true;
    },
    addToList(groupList) {
      let item_clone = Object.assign({}, this.itemToAdd);
      item_clone.created_at = Date();
      groupList.list.unshift(item_clone);
      this.finishAdding(groupList);
    },
    finishAdding(groupList) {
      groupList.adding = false;
      this.itemToAdd.name = '';
      this.itemToAdd.info = '';
      this.itemToAdd.details = '';
      this.shouldSave();
    },
    saveNotes() {
      var FileSaver = require('file-saver');

      var json = JSON.stringify(this.$data);
      var blob = new Blob([json], {
        type: 'text/plain;charset=utf-8'
      });
      FileSaver.saveAs(blob, 'S&S_notes.txt');
      this.save = false;
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
    },
    groupListCheck() {
      let show;
      this.groupList.map((item) => {
        if (item.list.length >= 1) {
          show = true;
        }
      });
      return show;
    }
  }
};
</script>
<style scoped>
.menu {
  padding: 30px;
}

.form-inputs {
  margin-left: 3px;
  display: flex;
  flex-direction: column;
}

input {
  color: #b1b1ff;
}
.form {
  margin-left: 16px;
}
.file-input {
  margin-top: 28px !important;
}
.list-item {
  padding: 10px;
}
.text-area {
  color: #b1b1ff;
  white-space: pre-wrap;
}
.detail-area {
  white-space: pre-wrap;
}
.btn-add {
  margin-top: 14px;
  margin-bottom: 14px;
}
textarea {
  width: 99%;
}
</style>
