<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>Notify</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true">
      <ion-grid>
        <ion-row>
          <ion-col>
            <ion-item>
              <ion-toggle
                v-model="notificationsEnabled"
                @ionChange="toggleNotifications"
                >Notify each morning</ion-toggle
              >
            </ion-item>
          </ion-col>
        </ion-row>
      </ion-grid>
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
  IonItem,
  IonCol,
  IonRow,
  IonToggle,
  IonGrid,
} from '@ionic/vue';
import { defineComponent } from 'vue';
import HolidayData from '../../public/assets/json/holidays.json';
import { LocalNotifications } from '@capacitor/local-notifications';
import moment from 'moment';
export default defineComponent({
  name: 'SearchPage',
  data() {
    return {
      term: '',
      HolidayData,
      notificationsEnabled: false,
    };
  },
  async ionViewDidEnter() {
    //check if they have any pending, if so set enabled to true
    LocalNotifications.getPending().then(
      (res) => {
        if (res.notifications.length) {
          console.log('enter found existing');
          this.notificationsEnabled = true;
        } else {
          console.log('enter found none');
          this.notificationsEnabled = false;
        }
      },
      (err) => {
        console.log(err);
      }
    );
  },
  methods: {
    async scheduleNotification(message: any, date: any, id: any) {
      const momentizedDate = moment(date, 'MMMM D');

      momentizedDate.set({
        hour: 8,
        minute: 0,
        second: 0,
      });

      if (momentizedDate.isBefore()) {
        momentizedDate.add(1, 'y');
      }

      LocalNotifications.schedule({
        notifications: [
          {
            title: 'Everyday Holiday Reminder',
            body: message,
            id,
            schedule: {
              at: new Date(momentizedDate.format()),
              repeats: true,
              every: 'year',
              count: 99,
            },
          },
        ],
      });
      console.log('scheduled notification');
    },
    async checkPermission() {
      const permission = await LocalNotifications.checkPermissions();
      if (permission.display != 'granted') {
        await LocalNotifications.requestPermissions();
      }
    },
    async toggleNotifications() {
      if (this.notificationsEnabled) {
        await this.checkPermission();

        const groupedMap = new Map();
        for (const e of HolidayData.holidays) {
          if (!groupedMap.has(e.date)) {
            groupedMap.set(e.date, []);
          }
          groupedMap.get(e.date).push(e);
        }

        let id = 1;
        groupedMap.forEach((h: any, i: any) => {
          id++;
          if (i !== '') {
            if (h.length == 1) {
              //   console.log('Today is ' + h[0].name, h[0].date, id);
              this.scheduleNotification('Today is ' + h[0].name, h[0].date, id);
            } else {
              let builtString = 'Todays is ';
              h.forEach((h: any, index: number) => {
                if (index == 0) {
                  builtString = builtString + h.name;
                } else {
                  builtString = builtString + ' & ' + h.name;
                }
              });
              //   console.log(builtString, h[0].date, id);
              this.scheduleNotification(builtString, h[0].date, id);
            }
          }
        });
      } else {
        LocalNotifications.getPending().then(
          (res) => {
            console.log('cancelling all', res.notifications);
            LocalNotifications.cancel(res);
          },
          (err) => {
            console.log(err);
          }
        );
      }
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
    IonItem,
    IonToggle,
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
