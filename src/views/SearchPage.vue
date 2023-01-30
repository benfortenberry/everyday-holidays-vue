<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>Search</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true">
      <ion-grid>
        <ion-row>
          <ion-col>
            <ion-item>
              <ion-label position="floating">Enter search term</ion-label>
              <ion-input @ionChange="doSearch" v-model="term"></ion-input>
            </ion-item>
          </ion-col>
        </ion-row>
      </ion-grid>

      <ion-list :inset="true" v-if="searchData.length && term">
        <ion-item
          class="ion-text-wrap"
          v-for="h in searchData"
          v-bind:key="h.keyword"
        >
          <ion-label class="ion-text-wrap">
            <h1>{{ h.name }}</h1>
            <p>{{ h.date }}</p>
          </ion-label>
        </ion-item>
      </ion-list>
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import {
  IonPage,
  IonHeader,
  IonToolbar,
  IonTitle,
  IonContent,
  IonLabel,
  IonItem,
  IonInput,
  IonCol,
  IonRow,
  IonGrid,
  IonList,
} from "@ionic/vue";
import { defineComponent } from "vue";
import HolidayData from "../../public/assets/json/holidays.json";
export default defineComponent({
  name: "SearchPage",
  data() {
    return {
      term: "",
      HolidayData,
      searchData: [{}],
      plusMinus: 0,
    };
  },

  methods: {
    doSearch() {
      console.log("do search");
      this.searchData.length = 0;
      this.HolidayData.holidays.forEach((it) => {
        if (it.name.toLowerCase().includes(this.term) && it.date) {
          this.searchData.push(it);
        }
      });
    },
  },
  components: {
    IonPage,
    IonCol,
    IonContent,
    IonList,
    IonRow,
    IonGrid,
    IonTitle,
    IonHeader,
    IonToolbar,
    IonLabel,
    IonItem,
    IonInput,
  },
});
</script>
