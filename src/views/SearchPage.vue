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
              <ion-input
                label="Find your holiday: "
                @ionInput="doSearch"
                v-model="term"
              ></ion-input>
            </ion-item>
          </ion-col>
        </ion-row>
      </ion-grid>
      <p v-if="term" class="ion-padding-start">
        Holidays found: {{ searchData.length }}
      </p>

      <ion-accordion-group v-if="term">
        <ion-accordion
          v-for="h in searchData"
          v-bind:key="h.keyword"
          :value="h.keyword"
        >
          <ion-item slot="header" color="tertiary">
            <ion-label>{{ h.name }} - {{ h.date }}</ion-label>
          </ion-item>
          <div class="border-top" slot="content">
            <ion-card class="ion-no-margin">
              <img :src="`assets/imgs/${h.keyword}.jpg`" />

              <ion-card-content class="white">
                {{ h.blurb }}
              </ion-card-content>
              <ion-button @click="doShare(h.name, h.date)" fill="clear"
                ><ion-icon color="dark" :icon="shareSocial"></ion-icon
              ></ion-button>
              <ion-button
                @click="scheduleNotification(h.name, h.date)"
                fill="clear"
                ><ion-icon color="dark" :icon="notifications"></ion-icon
              ></ion-button>
            </ion-card>
          </div>
        </ion-accordion>
      </ion-accordion-group>
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
  IonInput,
  IonCol,
  IonRow,
  IonCardContent,
  IonAccordion,
  IonAccordionGroup,
  IonGrid,
  IonCard,
  IonAlert,
  IonIcon,
  IonButton,
} from '@ionic/vue';
import { defineComponent } from 'vue';
import { shareSocial, notifications } from 'ionicons/icons';
import HolidayData from '../../public/assets/json/holidays.json';
import { Share } from '@capacitor/share';
import { LocalNotifications } from '@capacitor/local-notifications';

export default defineComponent({
  name: 'SearchPage',
  data() {
    return {
      term: '',
      HolidayData,
      searchData: [{}],
      shareSocial,
      notifications,
      isOpen: false,
      alertButtons: ['OK'],
      plusMinus: 0,
    };
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
    doSearch() {
      console.log('do search');
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
    IonRow,
    IonGrid,
    IonTitle,
    IonHeader,
    IonToolbar,
    IonCardContent,
    IonLabel,
    IonItem,
    IonAccordion,
    IonButton,
    IonAccordionGroup,
    IonInput,
    IonCard,
    IonAlert,
    IonIcon,
  },
});
</script>

<style scoped>
.white {
  color: white;
}

.border-top {
  border-top: solid #111 1px;
}

.border-bottom {
  border-bottom: solid #111 1px;
}
</style>
