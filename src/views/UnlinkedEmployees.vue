<template>
<div>
    <v-data-table :headers="headers" :items="unlinked" :search="search" sort-by="calories" class="elevation-1" :loading="loading" loading-text="Loading... Please wait">
        <template v-slot:top>
            <v-toolbar flat >
            <v-toolbar-title>List of all Employees</v-toolbar-title>

            <v-divider class="mx-4" inset vertical></v-divider>

            <v-btn elevation="0">
                {{filter}}
            </v-btn>

            <v-spacer></v-spacer>

            <v-text-field v-model="search" append-icon="mdi-magnify" label="Search this table" color="green" single-line hide-details></v-text-field>
            <v-divider class="mx-4" inset vertical></v-divider>
            
            <v-dialog v-model="dialog" max-width="500px">
                <template v-slot:activator="{ on, attrs }">
                    <v-btn color="green" dark class="mb-2" v-bind="attrs" v-on="on">
                        Register New Patient
                    </v-btn>
                </template>

                <v-card>
                    <v-card-title>
                    <span class="headline">{{ formTitle }}</span>
                    </v-card-title>

                    <v-card-text>
                        <v-container>
                            <v-text-field
                                v-model="editedItem.name"
                                label="Dessert name"
                                ></v-text-field>
                            <v-text-field
                                v-model="editedItem.calories"
                                label="Calories"
                                ></v-text-field>
                            <v-text-field
                                v-model="editedItem.fat"
                                label="Fat (g)"
                                ></v-text-field>
                            <v-text-field
                                v-model="editedItem.protein"
                                label="Protein (g)"
                                ></v-text-field>
                        </v-container>
                    </v-card-text>

                    <v-card-actions>
                        <v-spacer></v-spacer>
                        <v-btn color="green darken-1" text @click="close" >
                            Cancel
                        </v-btn>
                        <v-btn color="green darken-1" text @click="save" >
                            Save
                        </v-btn>
                    </v-card-actions>
          </v-card>
        </v-dialog>
        <v-dialog v-model="dialogDelete" max-width="500px">
          <v-card>
            <v-card-title class="headline">Are you sure you want to delete this item?</v-card-title>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue darken-1" text @click="closeDelete">Cancel</v-btn>
              <v-btn color="blue darken-1" text @click="deleteItemConfirm">OK</v-btn>
              <v-spacer></v-spacer>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-toolbar>
    </template>
    <template v-slot:[`item.actions`]="{ item }">
      <v-icon
        small
        class="mr-2"
        @click="editItem(item)"
      >
        mdi-pencil
      </v-icon>
      <v-icon
        small
        @click="deleteItem(item)"
      >
        mdi-delete
      </v-icon>
    </template>
    <template v-slot:no-data>
      <v-btn
        color="primary"
        @click="initialize"
      >
        Reset
      </v-btn>
    </template>
  </v-data-table>
  </div>
</template>

<script>
import axios from 'axios'
    export default {
        name: 'UnlinkedEmployees',
        data: () => ({
            search: '',
            alpha: ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J', 'K', 'L', 'M', 'N', 'O', 'P', 'Q', 'R', 'S', 'T', 'U', 'V', 'W', 'X', 'Y', 'Z'],
            filter: 'B',
            loading: true,
      dialog: false,
      dialogDelete: false,
      headers: [
        { text: 'Employee No.', value: 'employee_id' },
        { text: 'Last Name', value: 'lname' },
        { text: 'First Name', value: 'fname' },
        { text: 'Birthdate', value: 'bdate' },
        { text: 'Position', value: 'position' },
        { text: 'Department', value: 'department' },
        { text: 'Actions', value: 'actions', sortable: false },
      ],
      desserts: [],
      editedIndex: -1,
      editedItem: {
        employee_id: '',
        lname: '',
        fname: '',
        bdate: '',
        position: '',
        department: ''
      },
      defaultItem: {
        employee_id: '',
        lname: '',
        fname: '',
        bdate: '',
        position: '',
        department: ''
      },
      unlinked: []
    }),

    computed: {
      formTitle () {
        return this.editedIndex === -1 ? 'New Item' : 'Edit Item'
      },
    },

    watch: {
      dialog (val) {
        val || this.close()
      },
      dialogDelete (val) {
        val || this.closeDelete()
      },
    },

    created () {
      this.initialize()
    },

    methods: {
      initialize () {
            // Get employee data from DB
            axios.get(`http://localhost:8001/api/employees`)
                .then(res => {
                    Object.values(res.data).forEach(employee => {
                        if(employee.patient_id == null){
                            this.unlinked.unshift(employee);
                        }
                    })

                    this.loading = false;
                }).catch(err => {
                    console.log(err)
            })
      },

      editItem (item) {
        this.editedIndex = this.desserts.indexOf(item)
        this.editedItem = Object.assign({}, item)
        this.dialog = true
      },

      deleteItem (item) {
        this.editedIndex = this.desserts.indexOf(item)
        this.editedItem = Object.assign({}, item)
        this.dialogDelete = true
      },

      deleteItemConfirm () {
        this.desserts.splice(this.editedIndex, 1)
        this.closeDelete()
      },

      close () {
        this.dialog = false
        this.$nextTick(() => {
          this.editedItem = Object.assign({}, this.defaultItem)
          this.editedIndex = -1
        })
      },

      closeDelete () {
        this.dialogDelete = false
        this.$nextTick(() => {
          this.editedItem = Object.assign({}, this.defaultItem)
          this.editedIndex = -1
        })
      },

      save () {
        if (this.editedIndex > -1) {
          Object.assign(this.desserts[this.editedIndex], this.editedItem)
        } else {
          this.desserts.push(this.editedItem)
        }
        this.close()
      },
    },
    }
</script>