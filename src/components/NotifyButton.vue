<template>
  <ion-button
    @click="scheduleNotification(holiday.name, holiday.date, holiday.id)"
    fill="clear"
  >
    <ion-icon v-if="!isPending" :icon="notifications"></ion-icon>
    <ion-icon v-else :icon="notificationsOff"></ion-icon>
  </ion-button>

  <ion-alert
    :is-open="isOpen"
    header="Success"
    :message="alertMessage"
    :buttons="alertButtons"
    @didDismiss="isOpen = false"
  ></ion-alert>
</template>

<script lang="ts">
import { IonButton, IonIcon, IonAlert } from '@ionic/vue';
import { defineComponent } from 'vue';
import { notifications, notificationsOff } from 'ionicons/icons';
import { LocalNotifications } from '@capacitor/local-notifications';
import moment from 'moment';

export default defineComponent({
  name: 'NotifyButton',
  data() {
    return {
      date: '',
      homeData: [{}],
      notifications,
      notificationsOff,
      alertButtons: ['OK'],
      isOpen: false,
      isPending: false,
      alertMessage: 'A reminder has been set.',
    };
  },
  props: {
    holiday: { type: Object, default: null },
  },
  mounted() {
    LocalNotifications.getPending().then(
      (res) => {
        res.notifications = res.notifications.filter(
          (i) => i.id == this.holiday.id
        );
        if (res.notifications.length > 0) {
          this.isPending = true;
        } else {
          console.log('notification length zero');
        }
      },
      (err) => {
        console.log(err);
      }
    );
  },
  methods: {
    scheduleNotification(name: any, date: any, id: any) {
      if (!this.isPending) {
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
              body: 'Today is ' + name,
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

        this.isOpen = true;
        this.isPending = true;
      } else {
        LocalNotifications.getPending().then(
          (res) => {
            res.notifications = res.notifications.filter((i) => i.id == id);
            if (res.notifications.length > 0) LocalNotifications.cancel(res);
            else {
              console.log('notification length zero');
            }
          },
          (err) => {
            console.log(err);
          }
        );
        this.isOpen = true;
        this.alertMessage = 'Reminder cancelled.';
        this.isPending = false;
      }
    },
  },
  components: {
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
</style>
