<template>
  <v-data-table
    :headers="headers"
    :items="desserts"
    sort-by="calories"
    class="elevation-0"
  >
    <template v-slot:top>
      <v-toolbar
        flat
      >
        <v-toolbar-title>Управление обедами</v-toolbar-title>
        <v-divider
          class="mx-4"
          inset
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
              v-bind="attrs"
              v-on="on"
            >
              Добавить
            </v-btn>
          </template>
          <v-card>
            <v-card-title>
              <span class="text-h5">{{ formTitle }}</span>
            </v-card-title>

            <v-card-text>
              <v-container fluid>
                <v-row>
                  <v-col
                    cols="12"
                  >
                   <v-text-field
                      v-model="editedItem.title.uz"
                      label="Название обеда 🇺🇿"
                    ></v-text-field>
                   <v-text-field
                      v-model="editedItem.title.ru"
                      label="Название обеда 🇷🇺"
                    ></v-text-field>
                    <v-text-field
                      v-model="editedItem.title.en"
                      label="Название обеда 🇬🇧"
                    ></v-text-field>
                    <v-text-field
                      v-model="editedItem.price"
                      type="number"
                      label="Price"
                    ></v-text-field>
                    <v-text-field
                      v-model="editedItem.slug"
                      label="slug"
                    ></v-text-field>
                  </v-col>
                  <v-col :cols="editedItem.image ? 8 : 12">
                     <v-text-field
                      v-model="editedItem.image"
                      label="Image url"
                    ></v-text-field>
                  </v-col>
                  <v-col v-if="editedItem.image" :cols="4">
                    <v-img :src="editedItem.image"></v-img>
                  </v-col>
                </v-row>
              </v-container>
            </v-card-text>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn
                color="blue darken-1"
                text
                @click="close"
              >
                Отмена
              </v-btn>
              <v-btn
                color="blue darken-1"
                text
                @click="save"
              >
                Сохранить
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
        <v-dialog v-model="dialogDelete" max-width="420px">
          <v-card>
            <p class="text-center text-h6 align-center pt-8">Are you sure you want to delete this item?</p>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue darken-1" text @click="closeDelete">Отмена</v-btn>
              <v-btn color="red darken-1" text @click="deleteItemConfirm">OK</v-btn>
              <v-spacer></v-spacer>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-toolbar>
    </template>
    <template v-slot:item.image="{ item }">
      <v-img v-if="item.image" :src="item.image" height="70" class="m-1 rounded-lg my-2" width="70"></v-img>
    </template>
    <template v-slot:item.actions="{ item }">
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
</template>
<script>
export default {
  data: () => ({
    dialog: false,
    dialogDelete: false,
    headers: [
      { text: 'Name of food', value: 'title.ru' },
      { text: 'image', value: 'image' },
      { text: 'price', value: 'price' },
      { text: 'Actions', value: 'actions', sortable: false }
    ],
    desserts: [],
    editedIndex: -1,
    editedItem: {
      title: {
        uz: '',
        ru: '',
        en: ''
      },
      image: '',
      price: 18000,
      slug: 'manti'
    },
    defaultItem: {
      title: {
        uz: '',
        ru: '',
        en: ''
      },
      image: '',
      price: 18000,
      slug: ''
    }
  }),

  computed: {
    formTitle () {
      return this.editedIndex === -1 ? 'Добавить новый обед' : 'Изменить обед'
    }
  },

  watch: {
    dialog (val) {
      val || this.close()
    },
    dialogDelete (val) {
      val || this.closeDelete()
    },

    'editedItem.title.en' (value) {
      this.editedItem.slug = value.split(' ').join('-')
    }
  },

  created () {
    this.initialize()
  },

  methods: {
    getTypeColor (value) {
      switch (value) {
        case 'hr':
          return 'red lighten-1'
        case 'chief':
          return 'indigo lighten-2'
        case 'staff':
          return 'green lighten-2'
        case 'admin':
          return 'blue lighten-2'
        case 'intern':
          return 'orange lighten-2'

        default:
          return 'white lighten-4'
      }
    },
    initialize () {
      this.desserts = [
        {
          id: 'e89b2122-b039-4642-a332-d420d62d21fa',
          _id: 'e89b2122-b039-4642-a332-d420d62d21fa',
          title: {
            uz: 'Manti',
            ru: 'Manti',
            en: 'Manti'
          },
          image: '/url',
          price: 18000,
          slug: 'manti'
        },
        {
          id: 'e89b212sd2-b039-4642-a332-d420d62d21fa',
          _id: 'e89b212sd2-b039-4642-a332-d420d62d21fa',
          title: {
            uz: 'Norin',
            ru: 'Norin',
            en: 'Norin'
          },
          image: '/norin',
          price: 18000,
          slug: 'norin'
        }
      ]
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
    }
  }
}
</script>
