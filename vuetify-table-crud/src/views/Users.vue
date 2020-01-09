<template>
  <v-data-table
    :headers="headers"
    :items="users"
    :items-per-page="10"
    :footer-props="{
      showFirstLastPage: true,
      firstIcon: 'mdi-arrow-collapse-left',
      lastIcon: 'mdi-arrow-collapse-right',
      prevIcon: 'mdi-minus',
      nextIcon: 'mdi-plus'
    }"    
    class="elevation-1 user-table"
  >
    <template v-slot:top>
      <v-toolbar flat color="white">
        <h1>Users</h1>
        <div class="flex-grow-1"></div>

        <v-dialog v-model="dialog" max-width="600px">
          <template v-slot:activator="{ on }">
            <v-btn color="success" dark class="mb-2" v-on="on">New User</v-btn>
          </template>
          <v-card>
            <v-card-title>
              <span style="margin-bottom: 40px;" class="headline">{{ formTitle }}</span>
            </v-card-title>

            <v-card-text>
              <v-container>
                <v-row>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      style="width: 440px;"
                      v-model="editedItem.id"
                      label="User Id:"
                      outlined
                    ></v-text-field>
                    <v-text-field
                      style="width: 440px;"
                      v-model="editedItem.name"
                      label="User name:"
                      outlined
                    ></v-text-field>
                    <v-text-field
                      style="width: 440px;"
                      v-model="editedItem.surname"
                      label="User surname:"
                      outlined
                    ></v-text-field>
                    <v-text-field
                      style="width: 440px;"
                      v-model="editedItem.phone"
                      label="User phone:"
                      outlined
                    ></v-text-field>
                  </v-col>
                </v-row>

                <v-row>
                    <v-col cols="12" sm="6" md="4">
                    <v-dialog
                      ref="dialog"
                      v-model="modal"
                      :return-value.sync="editedItem.date_birthday"
                      persistent
                      width="290px"
                    >
                      <template v-slot:activator="{ on }">
                        <v-text-field
                          v-model="editedItem.date_birthday"
                          label="Picker in dialog"
                          prepend-icon="event"
                          readonly
                          v-on="on"
                        ></v-text-field>
                      </template>
                      <v-date-picker v-model="editedItem.date_birthday"  color="green lighten-1" scrollable>
                        <v-spacer></v-spacer>
                        <v-btn text  color="green lighten-1" @click="modal = false">Cancel</v-btn>
                        <v-btn text  color="green lighten-1" @click="$refs.dialog.save(editedItem.date_birthday)">OK</v-btn>
                      </v-date-picker>
                    </v-dialog>
                  </v-col>
                </v-row>

              </v-container>
            </v-card-text>

            <v-card-actions>
              <div class="flex-grow-1"></div>
                <div v-if="dialogDelete === true">
                  <v-btn color="blue darken-1" text @click="close">Cancel</v-btn>
                  <v-btn color="blue darken-1" text @click="remove">Delete</v-btn>
                </div>
                <div v-else-if="dialogDelete === false">
                  <v-btn color="blue darken-1" text @click="close">Cancel</v-btn>
                  <v-btn color="blue darken-1" text @click="save">Save</v-btn>
                </div>
            </v-card-actions>

          </v-card>
        </v-dialog>
      </v-toolbar>
    </template>

    <template v-slot:item.action="{ item }">
      <v-icon medium color="success" class="mr-2" @click="editItem(item)">edit</v-icon>
      <v-icon medium color="success" @click="deleteItem(item)">delete</v-icon>
    </template>

    <template v-slot:no-data>
      <v-btn color="primary" @click="initialize">Reset</v-btn>
    </template>
  </v-data-table>
</template>

<script>
import usersData from "../data/users.json";
export default {
  data: () => ({
    dialogDelete: false,
    dialog: false,
    headers: [
      { text: "Id", value: "id", width: "6%" },
      { text: "Name", align: "left", sortable: true, value: "name" },
      { text: "Surname", align: "left", sortable: true, value: "surname" },
      { text: "Phone", align: "left", sortable: true, value: "phone" },
      { text: "date of birthday", align: "left", sortable: true, value: "date_birthday" },
      { text: "Actions", value: "action", sortable: false, width: "8%" }
    ],
    date_birthday: new Date().toISOString().substr(0, 10),
    modal: false,
    
    users: [],
    editedIndex: -1,  // New User
    editedItem: {
      id: 0,
      name: "",
      surname: "",
      date_birthday: new Date().toISOString().substr(0, 10),
      phone: "",
    },
    defaultItem: {
      id: 0,
      name: "",
      surname: "",
      date_birthday: new Date().toISOString().substr(0, 10),
      phone: "",
    }
  }),

  computed: {
    formTitle() {
      if (this.dialogDelete) {
        return "Delete User";
      } else if (this.editedIndex === -1) {
        return "New User";
      } else if (this.editedIndex > -1) {
        return "Edit User";
      }  
    }
  },

  watch: {
    dialog(val) {
      val || this.close();
    }
  },

  created() {
    this.initialize();
  },

  methods: {
    initialize() {
      this.users = usersData;
    },

    editItem(item) {
      this.dialogDelete = false;
      this.editedIndex = this.users.indexOf(item);
      this.editedItem = Object.assign({}, item);
      this.dialog = true;
    },

    deleteItem(item) {
      this.dialogDelete = true;
      this.editedIndex = this.users.indexOf(item);
      this.editedItem = Object.assign({}, item);
      this.dialog = true;
    },

    close() {
      this.dialog = false;
      setTimeout(() => {
        this.editedItem = Object.assign({}, this.defaultItem);
        this.editedIndex = -1;
        this.dialogDelete = false;
      }, 300);
    },

    save() {
      if (this.editedIndex > -1) {  // Edited save
        Object.assign(this.users[this.editedIndex], this.editedItem);
      } else {  // New save
        this.users.push(this.editedItem);
      }
      this.close();
    },

    remove() {
      this.users.splice(this.editedIndex, 1);
      this.close();
    }
  }
};
</script>

<style>
.user-table {
    margin-top: 100px; 
    margin-left: 3%; 
    margin-right: 3%;
}
.user-table table th {
  background-color: lightgray;
  font-size: 20px !important;
}
</style>