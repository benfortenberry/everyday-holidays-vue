<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-buttons slot="start">
          <ion-button @click="subtractFromDate"
            ><ion-icon :icon="arrowBackSharp"></ion-icon
          ></ion-button>
        </ion-buttons>

        <ion-buttons slot="end">
          <ion-button @click="addToDate"
            ><ion-icon :icon="arrowForwardSharp"></ion-icon
          ></ion-button>
        </ion-buttons>

        <ion-title>{{ date }}</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content
      :fullscreen="true"
      v-touch:swipe.left="addToDate"
      v-touch:swipe.right="subtractFromDate"
    >
      <ion-grid :fixed="true">
        <ion-row>
          <ion-col size="12" size-sm="6" offset-sm="3">
            <holliday-card :holidayData="homeData" />
          </ion-col>
        </ion-row>
      </ion-grid>
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import {
  IonGrid,
  IonCol,
  IonContent,
  IonRow,
  IonTitle,
  IonHeader,
  IonToolbar,
  IonButton,
  IonButtons,
  IonIcon,
} from '@ionic/vue';
import { IonPage } from '@ionic/vue';
import { defineComponent } from 'vue';
import moment from 'moment';
import HolidayData from '../../public/assets/json/holidays.json';
import HollidayCard from '../components/HolidayCard.vue';
import { arrowForwardSharp, arrowBackSharp } from 'ionicons/icons';

export default defineComponent({
  name: 'HomePage',
  data() {
    return {
      date: '',
      HolidayData,
      homeData: [{}],
      arrowForwardSharp,
      arrowBackSharp,
      plusMinus: 0,
      startEvent: null,
      endEvent: null,
    };
  },
  async ionViewDidEnter() {
    this.date = moment().format('MMMM D');
    this.getHomeData();
  },

  methods: {
    getHomeData() {
      this.homeData.length = 0;
      HolidayData.holidays.forEach((element) => {
        if (element.date == this.date) {
          this.homeData.push(element);
        }
      });
    },
    subtractFromDate() {
      this.plusMinus--;
      this.date = moment().add(this.plusMinus, 'd').format('MMMM D');
      this.getHomeData();
    },
    addToDate() {
      this.plusMinus++;
      this.date = moment().add(this.plusMinus, 'd').format('MMMM D');
      this.getHomeData();
    },
  },
  components: {
    IonPage,
    IonGrid,
    IonCol,
    IonContent,
    IonRow,
    IonTitle,
    IonHeader,
    IonToolbar,
    IonButton,
    IonButtons,
    IonIcon,
    HollidayCard,
  },
});
</script>

<style scoped>
.white {
  color: white;
}
</style>
