<template>
  <v-container>
    <v-stepper v-model="step" vertical class="elevation-0">
      <v-stepper-step :step="1" :complete="step > 1">
        Auction {{ date ? `(${new Date(date).toDateString()})` : "" }}
      </v-stepper-step>
      <v-stepper-content :step="1">
        <v-card :loading="loading" elevation="0">
          <client-only>
            <v-list-item-group elevation="0" v-model="date" color="primary">
              <v-list-item
                v-for="{ date, count } in dates"
                :key="date"
                :value="date"
              >
                <v-list-item-content>
                  <v-list-item-title>
                    {{ new Date(date).toDateString() }}
                  </v-list-item-title>
                  <v-list-item-subtitle>{{ count }}</v-list-item-subtitle>
                </v-list-item-content>
              </v-list-item>
            </v-list-item-group>
          </client-only>
          <v-card-actions>
            <v-btn
              :disabled="!date"
              text
              large
              color="primary"
              @click="findMakers"
            >
              Continue
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-stepper-content>
      <v-stepper-step :step="2" :complete="step > 2">
        Maker {{ makersName.length ? `(${makersName})` : "" }}
      </v-stepper-step>

      <v-stepper-content :step="2">
        <v-card :loading="loading" elevation="0">
          <v-chip-group
            v-model="selected_makers"
            color="primary"
            multiple
            column
          >
            <v-chip
              v-for="maker in makers"
              :key="maker.MARKA_ID"
              :value="maker.MARKA_ID"
              class="px-5"
              filter
              outlined
            >
              {{ maker.MARKA_NAME }} ({{ maker["TAG2"] }})
            </v-chip>
          </v-chip-group>
          <v-card-actions>
            <v-btn text large color="primary" @click="findModels">
              Continue
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-stepper-content>
      <v-stepper-step :step="3" :complete="step > 3">
        Models
        {{ modelsName.length ? `(${modelsName})` : "" }}
      </v-stepper-step>
      <v-stepper-content :step="3">
        <v-progress-linear v-show="loading"></v-progress-linear>
        <v-item-group v-model="selected_models" multiple>
          <v-row>
            <v-col
              v-for="model in models"
              :key="model.MODEL_ID"
              class="fill-height"
              cols="6"
              sm="3"
              md="2"
            >
              <v-item v-slot="{ active, toggle }" :value="model.MODEL_ID">
                <v-card
                  elevation="0"
                  outlined
                  class="fill-height"
                  :class="[active ? 'primary--text' : '']"
                  @click="toggle"
                >
                  <v-img height="100px" :src="model.IMAGES.split('#')[0]">
                  </v-img>
                  <v-card-text
                    class="subtitle-2"
                    :class="[active ? 'primary--text' : '']"
                  >
                    {{ model["MODEL_NAME"] }} ( {{ model["TAG3"] }})
                  </v-card-text>
                </v-card>
              </v-item>
            </v-col>
          </v-row>
        </v-item-group>
        <v-btn class="mt-8" text large color="primary" @click="findCars">
          Continue
        </v-btn>
      </v-stepper-content>
      <v-stepper-step :step="4">
        Results
      </v-stepper-step>
      <v-stepper-content :step="4">
        <v-data-table :loading="loading" :headers="headers" :items="items">
          <template v-slot:[`item.photo`]="{ item }">
            <v-img
              :src="item.IMAGES.split('#')[0]"
              contain
              height="100px"
              max-width="150px"
            ></v-img>
          </template>
        </v-data-table>
        <v-btn class="mt-8" text large color="primary" @click="reset">
          Reset
        </v-btn>
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
    loading: false,
    selected_makers: [],
    dates: [],
    models: [],
    selected_models: [],
    items: [],
    headers: [
      {
        text: "Model",
        value: "MODEL_NAME"
      },
      {
        text: "photo",
        value: "photo"
      },
      {
        text: "year",
        value: "YEAR"
      },
      {
        text: "Color",
        value: "COLOR"
      },
      {
        text: "Avg. Price",
        value: "AVG_PRICE"
      },
      {
        text: "Auction",
        value: "AUCTION"
      },
      {
        text: "Auction Date",
        value: "AUCTION_DATE"
      }
    ]
  }),
  computed: {
    makersName() {
      const makers = this.makers
        .filter(maker => this.selected_makers.includes(maker.MARKA_ID))
        .map(maker => maker.MARKA_NAME.toLowerCase());
      return makers.join(", ");
    },
    modelsName() {
      const models = this.models
        .filter(model => this.selected_models.includes(model.MODEL_ID))
        .map(model => model.MODEL_NAME.toLowerCase());
      return models.join(", ");
    }
  },
  mounted() {
    this.getDates();
  },
  methods: {
    async getDates() {
      this.loading = true;
      try {
        const dates = await this.$axios.$get("/auctions/dates");
        this.dates = dates;
      } catch (error) {
        console.log(error);
      }
      this.loading = false;
    },
    async findMakers() {
      this.loading = true;
      this.step = 2;
      try {
        if (!this.date) this.step = 1;
        else {
          const data = await this.$axios.$get("/auctions/makers/" + this.date);
          this.makers = data;
        }
      } catch (error) {
        console.log(error);
      }
      this.loading = false;
    },
    async findModels() {
      this.loading = true;
      this.step = 3;
      try {
        if (!this.selected_makers.length) this.step = 2;
        else {
          const items = this.selected_makers.join(",");
          const data = await this.$axios.$get(
            `/auctions/models/${this.date}/${items}`
          );
          this.models = data;
        }
      } catch (error) {
        console.log(error);
      }
      this.loading = false;
    },
    async findCars() {
      this.loading = true;
      this.step = 4;
      try {
        if (!this.selected_models.length) this.step = 3;
        else {
          const items = this.selected_models.join(",");
          const data = await this.$axios.$get(
            `/auctions/cars/${this.date}/${items}`
          );
          this.items = data;
        }
      } catch (error) {
        console.log(error);
      }
      this.loading = false;
    },
    reset() {
      this.step = 1;
      (this.date = ""),
        (this.selected_makers = []),
        (this.selected_models = []),
        (this.items = []);
    }
  }
};
</script>
