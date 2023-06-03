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
    <ion-content :fullscreen="true">
      <ion-content>
        <ion-grid :fixed="true">
          <ion-row>
            <ion-col size="12" size-sm="6" offset-sm="3">
              <ion-card v-for="h in homeData" v-bind:key="h.keyword">
                <img :src="`assets/imgs/${h.keyword}.jpg`" />

                <ion-card-header text-wrap>
                  <ion-card-title>{{ h.name }}</ion-card-title>
                </ion-card-header>

                <ion-card-content class="white">
                  {{ h.blurb }}
                </ion-card-content>
                <ion-button @click="doShare(h.name, h.date)" fill="clear"
                  ><ion-icon color="dark" :icon="shareSocial"></ion-icon
                ></ion-button>
                <ion-button
                  @click="scheduleNotification(h.name, h.date)"
                  fill="clear"
                  ><ion-icon :icon="notifications"></ion-icon
                ></ion-button>
                <!-- <ion-button fill="clear">Add to Calendar</ion-button> -->
              </ion-card>
            </ion-col>
          </ion-row>
        </ion-grid>
      </ion-content>
    </ion-content>

    <ion-alert
      :is-open="isOpen"
      header="Success"
      message="A reminder will be sent every year."
      :buttons="alertButtons"
      @didDismiss="isOpen = false"
    ></ion-alert>
  </ion-page>
</template>

<script lang="ts">
import {
  IonGrid,
  IonCard,
  IonCol,
  IonContent,
  IonCardHeader,
  IonCardContent,
  IonRow,
  IonTitle,
  IonHeader,
  IonToolbar,
  IonButton,
  IonButtons,
  IonCardTitle,
  IonIcon,
  IonAlert,
} from '@ionic/vue';
import { IonPage } from '@ionic/vue';
import { defineComponent } from 'vue';
import moment from 'moment';
import HolidayData from '../../public/assets/json/holidays.json';
import {
  arrowForwardSharp,
  arrowBackSharp,
  shareSocial,
  notifications,
} from 'ionicons/icons';
import { Share } from '@capacitor/share';
import { LocalNotifications } from '@capacitor/local-notifications';

export default defineComponent({
  name: 'HomePage',
  data() {
    return {
      date: '',
      HolidayData,
      homeData: [{}],
      arrowForwardSharp,
      shareSocial,
      notifications,
      arrowBackSharp,
      plusMinus: 0,
      alertButtons: ['OK'],
      isOpen: false,
    };
  },
  async ionViewDidEnter() {
    this.date = moment().format('MMMM D');
    this.getHomeData();

    const permission = await LocalNotifications.checkPermissions();
    console.log(permission);

    await LocalNotifications.requestPermissions();
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
    scheduleNotification(name: any, date: any) {
      try {
        const dateParts = date.split(' ');

        const monthNum = new Date(`${dateParts[0]} 1, 2022`).getMonth() + 1;

        const id = new Date().getTime() / 1000;

        this.isOpen = true;

        LocalNotifications.schedule({
          notifications: [
            {
              title: 'Everyday Holiday Reminder',
              body: 'Today is ' + name,
              id,
              schedule: {
                repeats: true,
                every: 'year',
                on: { month: monthNum, day: dateParts[1] },
              },
            },
          ],
        });
      } catch {
        console.log('notification error');
      }
    },
    async doShare(holidayName: any) {
      try {
        await Share.share({
          text: this.date + ' is ' + holidayName,

          dialogTitle: 'Share holiday',
        });
        console.log(holidayName);
      } catch {
        console.log('error');
      }
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
    IonCard,
    IonCol,
    IonContent,
    IonCardHeader,
    IonRow,
    IonTitle,
    IonHeader,
    IonToolbar,
    IonButton,
    IonButtons,
    IonIcon,
    IonCardContent,
    IonCardTitle,
    IonAlert,
  },
});
</script>

<style scoped>
.white {
  color: white;
}
</style>
