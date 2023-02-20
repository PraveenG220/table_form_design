<template><div>
    <v-data-table 

      :headers="headers"
      :items="inputs"
      sort-by="code"
      class="elevation-2   blue-grey lighten-4"
      
    >
      <template v-slot:top>
        <v-toolbar
        class="blue lighten-2"
          flat
        >
          <v-toolbar-title>Section</v-toolbar-title>
          <v-divider
            class="mx-4"
            dark
            vertical
            ></v-divider>
          <v-spacer></v-spacer>
          <v-dialog
            v-model="dialog"
            max-width="500px" 
          
          >
            <template v-slot:activator="{ on, attrs }">
              <v-btn
                color="primary"
                dark
                class="mb-5"
                v-bind="attrs"
                v-on="on"
              >
                Add
              </v-btn>
            </template>
            <v-card>
              <v-card-title>
                <span class="text-h5">{{ formTitle }}</span>
              </v-card-title>
  
              <v-card-text>
                <v-container>
                  <v-row>
                    <v-col
                   
                      cols="12"
                      sm="6"
                     
                    >
                      <v-text-field
                        v-model="editedItem.fname"
                        label="firstname"
                        outlined
                        dense
                      ></v-text-field>
                    </v-col>
                    <v-col
                   
                      cols="12"
                      sm="6"
                     
                    >
                      <v-text-field
                        v-model="editedItem.lname"
                        label="lastname"
                        outlined
                        dense
                      ></v-text-field>
                    </v-col>
                    <v-col
                   
                      cols="12"
                      sm="6"
                     
                    >
                      <v-text-field
                        v-model="editedItem.ecode"
                        label="employee code"
                        outlined
                        dense
                      ></v-text-field>
                    </v-col>

                    <v-col
                      cols="12"
                      sm="6"
                     
                    >
                     <v-text-field
                        v-model="editedItem.place"
                        label="Place"
                        outlined
                         dense
                      ></v-text-field>
                    </v-col>
                  </v-row>
                </v-container>
              </v-card-text>
  
              <v-card-actions >
                <v-spacer></v-spacer>
                <v-btn 
                class="pa-2"
                  color="blue darken-1"
                  text
                  @click="close"
                >
                  Cancel
                </v-btn>
                <v-btn
                  color="blue darken-1"
                  text
                  @click="save"
                >
                  Save
                </v-btn>
              </v-card-actions>
             </v-card>
          </v-dialog>
          <v-dialog v-model="dialogDelete" max-width="600px">
            <v-card>
              <v-card-title class="text-h5">Are you sure you want to delete this details?</v-card-title>
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
      <template v-slot:[`item.actions`]= "{ item }" >
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
    {{ output }}
  </div>
  </template>

<script>
export default {
  data: () => ({
    dialog: false,
    dialogDelete: false,
    headers: [
      { text: ' firstname', align: 'start', sortable: false,  value: 'fname',},
      { text: 'lastname', align: 'start',sortable: false, value: 'lname', },
      { text: 'employee code', align: 'start', sortable: false,  value: 'ecode',},
      { text: 'place', align: 'start',sortable: false, value: 'place', },
      { text: 'Actions', sortable: false, value: 'actions', }
    ],
    inputs: [],
    editedIndex: -1,
    editedItem: {
        fname: '',
        lname: '',
        ecode: '',
        place: '',
        
    },
    defaultItem: {
        
        fname: '',
        lname: '',
        ecode: '',
        place: '',
    },
     output:''
  }),

  computed: {
    formTitle () {
      return this.editedIndex === -1 ? 'Add text' : 'Edit text'
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
    // this.getAllUsers()
    this.inputs= [{
        
        fname: 'Abhishek',
        lname: 'Nair',
        ecode: 'EMP001',
        place: 'Hubli',
    },{
        
        fname: 'Arun',
        lname: 'K',
        ecode: 'EMP002',
        place: "Mandya",
    }
  ]
  },

  methods: {
   async getAllUsers () {
    try {
           
        let response = await this.$axios.$get(
            `/getdata`
          )
          if(response)
          this.inputs = response
         } catch (error) {
            console.log('Catch error', error)
         }
    },

    editItem (item) {
      this.editedIndex = this.inputs.indexOf(item)
      this.editedItem = Object.assign({}, item)
      this.dialog = true
    },

    deleteItem (item) {
      this.editedIndex = this.inputs.indexOf(item)
      this.editedItem = Object.assign({}, item)
      this.dialogDelete = true
    },

   async deleteItemConfirm () {
      try {
           
           let response = await this.$axios.$delete(
               `/deletedata/${this.editedItem?._id}`
             )
            
             this.getAllUsers()
            } catch (error) {
               console.log('Catch error', error)
            }
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

   async save () {
      if (this.editedIndex > -1) {
        
         try {
            let payload= {...this.editedItem}
        let response = await this.$axios.$put(
            `/updatedata/${payload._id}`,
            payload
          )
          if(response){
            console.log("Updated successfully")
            this.getAllUsers()
          }
         } catch (error) {
            console.log('Catch error', error)
         }
      } else {
         
         try {
            let payload= {...this.editedItem}
        let response = await this.$axios.$post(
            `/add`,
            payload
          )
          if(response){
            console.log("added successfully")
            this.getAllUsers()
          }
         } catch (error) {
            console.log('Catch error', error)
         }
       
      }
      this.close()
    },
  },
}
</script>