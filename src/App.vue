<template>
  <v-app id="sandbox">
    <v-navigation-drawer
      v-model="primaryDrawer.model"
      :clipped="primaryDrawer.clipped"
      :floating="primaryDrawer.floating"
      :mini-variant="primaryDrawer.mini"
      :permanent="primaryDrawer.type === 'permanent'"
      :temporary="primaryDrawer.type === 'temporary'"
      app
      overflow
    />

    <v-app-bar :clipped-left="primaryDrawer.clipped" app class="green">
      <v-app-bar-nav-icon
        v-if="primaryDrawer.type !== 'permanent'"
        @click.stop="primaryDrawer.model = !primaryDrawer.model"
      />
      <v-toolbar-title>auto.ria API</v-toolbar-title>
      <v-spacer />
      <!-- <v-navigation-drawer> -->
      <v-switch v-model="$vuetify.theme.dark" primary class="mt-5" />
      <!-- </v-navigation-drawer> -->
      <!-- <v-toolbar-title>
        
      </v-toolbar-title>-->
    </v-app-bar>

    <v-content class="d-flex justify-center align-center">
      <v-container fluid>
        <v-row align="center" justify="center">
          <v-col cols="10">
            <v-card class="mx-auto" :elevation="24">
              <v-card-text>
                <v-form ref="form">
                  <v-row>
                    <v-col cols="6" sm="6" xs="6">
                      <v-autocomplete
                        v-model="brand"
                        :rules="nameRules"
                        :items="info"
                        item-text="name"
                        item-value="name"
                        :return-object="checkTrue"
                        dense
                        label="Марка"
                      ></v-autocomplete>
                    </v-col>
                    <v-col cols="6" sm="6" xs="6">
                      <v-autocomplete
                        v-model="inputsObject.model"
                        :rules="nameRules"
                        :items="models"
                        item-text="name"
                        item-value="name"
                        dense
                        label="Модель"
                      ></v-autocomplete>
                    </v-col>
                  </v-row>
                </v-form>
              </v-card-text>
              <v-card-actions>
                <div class="hidden-res">
                  <p></p>
                </div>
                <div class="flex-between">
                  <!-- <v-btn rounded @click="reset" class="reset">Reset</v-btn> -->
                </div>
              </v-card-actions>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-content>

    <v-footer :inset="footer.inset" app class="black white--text">
      <span class="px-4">&copy; {{ new Date().getFullYear() }}</span>
    </v-footer>
  </v-app>
</template>

<script>
import axios from "axios";
export default {
  data: () => ({
    primaryDrawer: {
      model: null,
      type: "default (no property)",
      clipped: false,
      floating: false,
      mini: false
    },
    footer: {
      inset: false
    },
    inputsObject: {
      brand: "",
      model: "",
      name: "",
      email: "",
      phone: ""
    },
    brand: {},
    nameRules: [v => !!v || "Обязательное поле"],
    response: {},
    value: "",
    marks: [],
    info: null,
    models: [],
    checkTrue: true
  }),
  mounted: function() {
    this.$nextTick(function() {
      axios
        .get(
          "https://developers.ria.com/auto/categories/1/marks?api_key=dbUQdiOGDFx1gIgcsIppZdrusE4AnA0lbrArUcAz"
        )
        .then(({ data }) => {
          console.log(data);
          this.info = data;
          console.log(this.info);
        })
        .catch(({ response }) => {
          console.log(response);
        });
    });
  },
  methods: {
    // reset: function() {
    //   if (confirm("Do you really want reset this form?")) {
    //     this.$refs.form.reset();
    //     console.log("reset:");
    //     console.log(this.inputsObject);
    //   }
    // }
  },
  watch: {
    "brand.value": function() {
      console.log(this.brand.value);
      axios
        .get(
          `http://api.auto.ria.com/categories/1/marks/${this.brand.value}/models?api_key=dbUQdiOGDFx1gIgcsIppZdrusE4AnA0lbrArUcAz`
        )
        .then(({ data }) => {
          console.log(data);
          this.models = data;
          console.log(this.models);
        })
        .catch(({ response }) => {
          console.log(response);
        });
    }
  }
};
</script>