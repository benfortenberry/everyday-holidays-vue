<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>All Holidays ({{ HolidayData.holidays.length }})</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true">
      <ion-spinner v-if="loading" name="crescent"></ion-spinner>

      <ion-list v-if="!loading" class="ion-no-padding">
        <ion-item-group
          class="border-bottom"
          v-for="h in HolidayData.holidays"
          v-bind:key="h.keyword"
        >
          <ion-item-divider v-if="!h.date">
            <ion-label>
              <h1 class="white">{{ h.name }}</h1>
            </ion-label>
          </ion-item-divider>

          <ion-item v-if="h.date" color="secondary">
            <ion-label class="ion-text-wrap">
              <h1>{{ h.name }}</h1>
              <p>{{ h.date }}</p>
            </ion-label>
            <ion-button @click="doShare(h.name, h.date)" fill="clear"
              ><ion-icon color="dark" :icon="shareSocial"></ion-icon
            ></ion-button>
            <ion-button
              @click="scheduleNotification(h.name, h.date)"
              fill="clear"
              ><ion-icon color="dark" :icon="notifications"></ion-icon
            ></ion-button>
          </ion-item>
        </ion-item-group>
      </ion-list>
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
  IonSpinner,
  IonButton,
  IonIcon,
  IonAlert,
} from '@ionic/vue';
import { defineComponent } from 'vue';
import { Share } from '@capacitor/share';
import { shareSocial, notifications } from 'ionicons/icons';
import HolidayData from '../../public/assets/json/holidays.json';
import { LocalNotifications } from '@capacitor/local-notifications';

export default defineComponent({
  name: 'AllPage',
  data() {
    return {
      term: '',
      HolidayData,
      shareSocial,
      notifications,
      isOpen: false,
      loading: true,
      alertButtons: ['OK'],
      plusMinus: 0,
    };
  },
  ionViewDidEnter() {
    this.loading = false;
  },
  methods: {
    async doShare(holidayName: any, date: any) {
      try {
        await Share.share({
          text: date + ' is ' + holidayName,

          dialogTitle: 'Share holiday',
        });
        console.log(holidayName);
      } catch {
        console.log('error');
      }
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
  },
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
    IonSpinner,
    IonButton,
    IonIcon,
    IonAlert,
  },
});
</script>

<style scoped>
.white {
  color: white;
}

.border-bottom {
  border-bottom: solid #111 1px;
}
</style>
