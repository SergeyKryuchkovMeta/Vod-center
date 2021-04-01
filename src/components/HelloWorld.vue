<template>
  <div>
    <div>
      <v-tabs fixed-tabs>
        <v-tab @click="handlerClick(i)" v-for="(title, i) in titles" :key="i">
          {{ title }}
        </v-tab>
      </v-tabs>
      <v-container fluid grid-list-sm>
        <v-layout class="justify-center">
          <v-flex xs8 sm10>
            <v-img
              max-height="300px"
              :src="
                show.picture
                  ? show.picture
                  : 'https://images.51microshop.com/1818/product/20180725/Request_A_Sample_1532534180500_0.jpg'
              "
              alt="no image"
            >
            </v-img>
          </v-flex>
        </v-layout>
      </v-container>
    </div>
    <div class="d-flex">
      <v-flex xs8 s8 offset-sm1>
        <template v-for="item in items[page]">
          <v-card
            :key="item.title"
            @click="checkItem(item)"
            class="card"
            :style="
              item.title === show.title
                ? { border: '2px solid rgba(166, 189, 252, 1)' }
                : { border: '2px solid white' }
            "
          >
            <v-list-item :key="item.title">
              <button class="item-button">
                <span class="item-number">
                  <v-icon
                    :style="
                      item.title === show.title
                        ? { color: 'blue' }
                        : { color: 'gray' }
                    "
                    class="icon"
                    >menu</v-icon
                  >
                  {{ item.number }}.
                </span>
              </button>

              <v-list-item-content class="ml-2">
                <v-list-item-title
                  v-html="item.title"
                  class="card-title"
                ></v-list-item-title>
                <v-list-item-subtitle
                  v-html="item.subtitle"
                  class="card-subtitle"
                ></v-list-item-subtitle>
              </v-list-item-content>
            </v-list-item>
          </v-card>
        </template>
      </v-flex>
      <v-flex xs4 sm4 ml-4>
        <div class="button-container">
          <v-radio-group mandatory v-model="user" color="green">
            <v-radio
              label="Stretch image"
              value="Value 1"
              color="success"
            ></v-radio>
            <v-radio
              label="Crop image"
              value="Value 2"
              color="success"
            ></v-radio>
            <v-radio
              label="Adjust manually"
              value="Value 3"
              color="success"
            ></v-radio>
          </v-radio-group>
          <v-dialog v-model="replaceDialog" max-width="600px">
            <v-btn
              slot="activator"
              color="success"
              v-show="!this.show.linkedObject.title"
              >Linked to</v-btn
            >
            <v-card>
              <v-card-title class="headline">Link to</v-card-title>
              <v-card-text>
                <v-layout column>
                  <v-text-field
                    hide-details
                    label="Type to search"
                    solo
                    v-model="query"
                  />
                  <v-list one-line>
                    <template v-for="folder in filteredFolders">
                      <v-card :key="folder.number" class="card">
                        <v-list-tile :key="folder.title">
                          <v-icon outlined color="blue">folder_open</v-icon>

                          <v-list-tile-content>
                            <button @click="changeSrc">
                              <v-list-tile-title
                                v-html="folder.title"
                                class="card-title"
                              ></v-list-tile-title>
                            </button>
                          </v-list-tile-content>
                        </v-list-tile>
                      </v-card>
                    </template>
                  </v-list>
                </v-layout>
              </v-card-text>
            </v-card>
          </v-dialog>
          <div class="linked-area" v-show="show.linkedObject.title != ''">
            <v-card class="mx-auto" max-width="344" width="344" outlined>
              <v-card-title primary-title>
                <h6 class="headline mb-0" style="color: #64b5f6">Linked</h6>
              </v-card-title>
              <v-card-text>
                <h6 class="headline mb-0">
                  Title: <small>{{ show.linkedObject.title }}</small>
                </h6></v-card-text
              >
              <v-card-text>
                <h6 class="headline mb-0">
                  Path: <small>{{ show.linkedObject.path }}</small>
                </h6></v-card-text
              >
              <v-card-actions>
                <v-btn @click.stop="replaceDialog = true" color="success"
                  >Change link</v-btn
                >
              </v-card-actions>
            </v-card>
          </div>
        </div>
      </v-flex>
    </div>
  </div>
</template>

<style>
.linked-area {
  margin: 10px;
}
.item-button {
  outline: none;
}
.button-container {
  max-width: 50%;
  display: flex;
  flex-direction: column;
}
.checkbox-area {
  align-items: center;
  margin: 10px;
  display: flex;
}
.checkbox-text {
  padding-left: 10px;
}
.checkbox {
  width: 20px;
  height: 20px;
}
.main {
  display: flex;
  justify-content: start !important;
}
.checkbox-text {
  font-size: 18px;
}
.button-list {
  list-style-type: none;
}
.item-number {
  display: flex;
  flex-direction: row;
}
.card-title {
  font-weight: 800;
}
.card-subtitle {
  color: black !important;
  font-weight: 700 !important;
  background-color: rgba(166, 189, 252, 0.25);
}
.icon {
  margin-right: 8px;
}
.card {
  margin: 5px;
  border: 1px solid rgb(88, 98, 100);
  border-radius: 2%;
}
::-webkit-scrollbar {
  width: 5px;
}
::-webkit-scrollbar-track {
  -webkit-box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.3);
}
::-webkit-scrollbar-thumb {
  background: rgba(97, 144, 230, 0.8);
}
.list {
  list-style-type: none;
  border-radius: 3%;
  padding: 10px;
  max-height: 520px;
  overflow-y: auto;
  display: flex !important;
  flex-direction: column;
  flex-grow: 1;
}
.title {
  padding-bottom: 10px;
  font-size: 30px !important;
}
</style>
<script>
export default {
  methods: {
    handlerClick(page) {
      this.page = page;
      this.show = this.items[page][0];
    },
    checkItem(item) {
      this.show.title === item.title
        ? (this.show = {
            ...item,
            title: "",
            picture: "",
            linkedObject: { title: "" },
          })
        : (this.show = item);
    },
    changeSrc(event) {
      if (this.show.title) {
        this.show.linkedObject.title = this.show.title;
        this.show.linkedObject.path = event.target.innerText;
      }
    },
  },
  computed: {
    filteredFolders() {
      if (this.query) {
        const filtered = [];
        this.folders.forEach((folder) => {
          if (
            folder.title !== null &&
            folder.title.match(new RegExp(this.query, "i"))
          ) {
            filtered.push(folder);
          }
        });
        return filtered;
      }
      return this.folders;
    },
  },
  data() {
    return {
      linkedObject: {
        title: "",
        path: "test/path",
      },
      page: 0,
      replaceDialog: false,
      show: {
        title: "Test title 1 for page series",
        subtitle: "Test subtitle 1 for page series",
        number: 1,
        picture: "https://wallpaper.dog/large/5509720.jpg",
        linkedObject: {
          title: "",
          path: "test/path",
        },
      },
      query: "",
      user: "",
      titles: ["SERIES", "MOVIES", "CHILDREN"],
      folders: [
        { number: 1, title: "Test 1" },
        { number: 2, title: "Test 2" },
        { number: 3, title: "Test 3" },
        { number: 4, title: "Test 4" },
        { number: 5, title: "Test 5" },
        { number: 6, title: "Test 6" },
        { number: 7, title: "Test 7" },
      ],
      items: [
        [
          {
            title: "Test title 1 for page series",
            subtitle: "Test subtitle 1 for page series",
            number: 1,
            picture: "https://wallpaper.dog/large/5509720.jpg",
            linkedObject: {
              title: "",
              path: "test/path",
            },
          },
          {
            title: "Test title 2 for page series",
            subtitle: "Test subtitle 2 for page series",
            number: 2,
            picture: "https://wallpaper.dog/large/5509723.jpg",
            linkedObject: {
              title: "",
              path: "test/path",
            },
          },
          {
            title: "Test title 3 for page series",
            subtitle: "Test subtitle 3 for page series",
            number: 3,
            picture: "https://wallpaper.dog/large/5509741.jpg",
            linkedObject: {
              title: "",
              path: "test/path",
            },
          },
          {
            title: "Test title 5 for page series",
            subtitle: "Test subtitle  for page series",
            number: 4,
            picture: "https://wallpaper.dog/large/5509743.jpg",
            linkedObject: {
              title: "",
              path: "test/path",
            },
          },
          {
            title: "Test title 6 for page series",
            subtitle: "Test subtitle 6 for page series",
            number: 5,
            picture: "https://wallpaper.dog/large/5509744.jpg",
            linkedObject: {
              title: "",
              path: "test/path",
            },
          },
          {
            title: "Test title 7 for page series",
            subtitle: "Test subtitle 7 for page series",
            number: 6,
            picture: "https://wallpaper.dog/large/5509766.jpg",
            linkedObject: {
              title: "",
              path: "test/path",
            },
          },
          {
            title: "Test title 8 for page series",
            subtitle: "Test subtitle 8 for page series",
            number: 7,
            picture: "https://wallpaper.dog/large/5509776.jpg",
            linkedObject: {
              title: "",
              path: "test/path",
            },
          },
        ],
        [
          {
            title: "Test title 1 for page movies",
            subtitle: "Test subtitle 4 for page movies",
            number: 1,
            picture: "https://wallpaper.dog/large/5509847.jpg",
            linkedObject: {
              title: "",
              path: "test/path",
            },
          },
          {
            title: "Test title 2 for page movies",
            subtitle: "Test subtitle 2 for page movies",
            number: 2,
            picture: "https://wallpaper.dog/large/5509823.jpg",
            linkedObject: {
              title: "",
              path: "test/path",
            },
          },
        ],
        [
          {
            title: "Test title 1 for page children",
            subtitle: "Test subtitle 1 for page children",
            number: 1,
            picture: "https://wallpaper.dog/large/5509830.jpg",
            linkedObject: {
              title: "",
              path: "test/path",
            },
          },
          {
            title: "Test title 2 for page children",
            subtitle: "Test subtitle 2 for page children",
            number: 2,
            picture: "https://wallpaper.dog/large/5509838.jpg",
            linkedObject: {
              title: "",
              path: "test/path",
            },
          },
          {
            title: "Test title 3 for page children",
            subtitle: "Test subtitle 3 for page children",
            number: 3,
            picture: "https://wallpaper.dog/large/5509842.jpg",
            linkedObject: {
              title: "",
              path: "test/path",
            },
          },
        ],
      ],
    };
  },
};
</script>
