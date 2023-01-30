<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>All Holidays</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true">
      <ion-list>
        <ion-item-group
          v-for="h in HolidayData.holidays"
          v-bind:key="h.keyword"
        >
          <ion-item-divider v-if="!h.date">
            <ion-label color="primary">
              <h1>{{ h.name }}</h1>
            </ion-label>
          </ion-item-divider>

          <ion-item v-if="h.date">
            <ion-label class="ion-text-wrap">
              <h1>{{ h.name }}</h1>
              <p>{{ h.date }}</p>
            </ion-label>
          </ion-item>
        </ion-item-group>
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
  IonItemDivider,
  IonItemGroup,
  IonList,
} from "@ionic/vue";
import { defineComponent } from "vue";
import HolidayData from "../../public/assets/json/holidays.json";
export default defineComponent({
  name: "AllPage",
  data() {
    return {
      term: "",
      HolidayData,

      plusMinus: 0,
    };
  },

  methods: {},
  components: {
    IonPage,
    IonItemDivider,
    IonContent,
    IonList,
    IonTitle,
    IonHeader,
    IonToolbar,
    IonLabel,
    IonItem,
    IonItemGroup,
  },
});
</script>
