<template>
  <v-container>
    <v-stepper v-model="step" vertical class="elevation-0">
      <v-stepper-step :step="1" :complete="step > 1">
        Auction {{ date ? `(${date})` : "" }}
      </v-stepper-step>
      <v-stepper-content :step="1">
        <v-card elevation="0">
          <v-card-text>
            <v-list-item-group elevation="0" v-model="date" color="primary">
              <v-list-item value="10 January 2021">
                <v-list-item-content>
                  <v-list-item-title>
                    10 January 2021, Saturday
                  </v-list-item-title>
                  <v-list-item-subtitle>
                    1220
                  </v-list-item-subtitle>
                </v-list-item-content>
              </v-list-item>
              <v-list-item>
                <v-list-item-content>
                  <v-list-item-title>
                    9 January 2021, Saturday
                  </v-list-item-title>
                  <v-list-item-subtitle>
                    1220
                  </v-list-item-subtitle>
                </v-list-item-content>
              </v-list-item>
              <v-list-item value="9 January 2021">
                <v-list-item-content>
                  <v-list-item-title>
                    8 January 2021, Saturday
                  </v-list-item-title>
                  <v-list-item-subtitle>
                    1220
                  </v-list-item-subtitle>
                </v-list-item-content>
              </v-list-item>
              <v-list-item value="8 January 2021">
                <v-list-item-content>
                  <v-list-item-title>
                    8 January 2021, Saturday
                  </v-list-item-title>
                  <v-list-item-subtitle>
                    1220
                  </v-list-item-subtitle>
                </v-list-item-content>
              </v-list-item>
            </v-list-item-group>
          </v-card-text>
          <v-card-actions>
            <v-btn :disabled="!date" text large color="primary" @click="step++">
              Continue
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-stepper-content>
      <v-stepper-step :step="2" :complete="step > 2">
        Maker {{ makers.length ? `(${makers.join(", ")})` : "" }}
      </v-stepper-step>

      <v-stepper-content :step="2">
        <v-card elevation="0">
          <h2 class="title">Japan</h2>
          <v-chip-group v-model="makers" color="primary" multiple column>
            <v-chip value="Honda" class="px-5" filter large outlined>
              <v-avatar size="40" left>
                <v-img src="/maker-2.png"></v-img>
              </v-avatar>
              Honda (200)
            </v-chip>
            <v-chip value="Nissan" class="px-5" filter large outlined>
              <v-avatar size="40" left>
                <v-img src="/maker-6.png"></v-img>
              </v-avatar>
              Nissan (250)
            </v-chip>
            <v-chip value="Suzuki" class="px-5" filter large outlined>
              <v-avatar size="40" left>
                <v-img src="/maker-8.png"></v-img>
              </v-avatar>
              Suzuki (120)
            </v-chip>
          </v-chip-group>
          <v-card-actions>
            <v-btn text large color="primary" @click="step++">
              Continue
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-stepper-content>
      <v-stepper-step :step="3" :complete="step > 3">
        Models {{ models.length ? `(${models.join(", ")})` : "" }}
      </v-stepper-step>
      <v-stepper-content :step="3">
        <v-item-group v-model="models" multiple>
          <v-row>
            <v-col class="fill-height" cols="6" sm="3" md="2">
              <v-item v-slot="{ active, toggle }" value="ad-expert">
                <v-card
                  elevation="0"
                  outlined
                  class="fill-height"
                  :class="[active ? 'primary--text' : '']"
                  @click="toggle"
                >
                  <v-img max-height="100px" contain src="/ad-expert.jpg">
                  </v-img>
                  <v-card-text
                    class="subtitle-2"
                    :class="[active ? 'primary--text' : '']"
                  >
                    Ad Expert (2)
                  </v-card-text>
                </v-card>
              </v-item>
            </v-col>
            <v-col class="fill-height" cols="6" sm="3" md="2">
              <v-item v-slot="{ active, toggle }" value="Caravan">
                <v-card
                  elevation="0"
                  outlined
                  class="fill-height"
                  :class="[active ? 'primary--text' : '']"
                  @click="toggle"
                >
                  <v-img max-height="100px" contain src="/caravan-coach.jpg">
                  </v-img>
                  <v-card-text
                    class="subtitle-2"
                    :class="[active ? 'primary--text' : '']"
                  >
                    Caravan (20)
                  </v-card-text>
                </v-card>
              </v-item>
            </v-col>
            <v-col class="fill-height" cols="6" sm="3" md="2">
              <v-item v-slot="{ active, toggle }" value="Dualis">
                <v-card
                  elevation="0"
                  outlined
                  class="fill-height"
                  :class="[active ? 'primary--text' : '']"
                  @click="toggle"
                >
                  <v-img max-height="100px" contain src="/dualis.jpg"> </v-img>
                  <v-card-text
                    class="subtitle-2"
                    :class="[active ? 'primary--text' : '']"
                  >
                    Dualis (20)
                  </v-card-text>
                </v-card>
              </v-item>
            </v-col>
            <v-col class="fill-height" cols="6" sm="3" md="2">
              <v-item v-slot="{ active, toggle }" value="Gloria">
                <v-card
                  elevation="0"
                  outlined
                  class="fill-height"
                  :class="[active ? 'primary--text' : '']"
                  @click="toggle"
                >
                  <v-img max-height="100px" contain src="/gloria.jpg"> </v-img>
                  <v-card-text
                    class="subtitle-2"
                    :class="[active ? 'primary--text' : '']"
                  >
                    Gloria (20)
                  </v-card-text>
                </v-card>
              </v-item>
            </v-col>
          </v-row>
        </v-item-group>
        <v-btn class="mt-8" text large color="primary" @click="step++">
          Continue
        </v-btn>
      </v-stepper-content>
      <v-stepper-step :step="4">
        Results
      </v-stepper-step>
      <v-stepper-content :step="4">
        <v-data-table :headers="headers" :items="items">
          <template v-slot:item.photo="{ item }">
            <v-img
              :src="item.photo"
              contain
              height="100px"
              max-width="150px"
            ></v-img>
          </template>
        </v-data-table>
      </v-stepper-content>
    </v-stepper>
  </v-container>
</template>
<script>
export default {
  name: "Auction",
  data: () => ({
    step: 1,
    date: "",
    makers: [],
    models: [],
    items: [
      {
        num: 1,
        photo:
          "https://p3.aleado.com/pic/?system=auto&date=2021-01-08&auct=69&bid=2328&number=1&h=320",
        model: "Caravan Coach",
        date: "25-07-2021",
        location: "Bayside 2328"
      },
      {
        num: 2,
        photo:
          "https://p3.aleado.com/pic/?system=auto&date=2021-01-08&auct=69&bid=2328&number=1&h=320",
        model: "Caravan Coach",
        date: "25-07-2021",
        location: "Bayside 2328"
      }
    ],
    headers: [
      {
        text: "number",
        value: "num"
      },
      {
        text: "photo",
        value: "photo"
      },
      {
        text: "Model",
        value: "model"
      },
      {
        text: "Date",
        value: "date"
      },
      {
        text: "Location",
        value: "location"
      }
    ]
  }),
  created() {},
  methods: {
    selectDate() {
      this.date = "8 January 2021, Saturday";
      this.step = 2;
    }
  }
};
</script>
