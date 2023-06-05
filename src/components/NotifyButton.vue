<template>
  <ion-button
    @click="scheduleNotification(holiday.name, holiday.date)"
    fill="clear"
    ><ion-icon :icon="notifications"></ion-icon
  ></ion-button>

  <ion-alert
    :is-open="isOpen"
    header="Success"
    message="A reminder has been set!"
    :buttons="alertButtons"
    @didDismiss="isOpen = false"
  ></ion-alert>
</template>

<script lang="ts">
import { IonButton, IonIcon, IonAlert } from '@ionic/vue';
import { defineComponent } from 'vue';
import { notifications } from 'ionicons/icons';
import { LocalNotifications } from '@capacitor/local-notifications';
import moment from 'moment';

export default defineComponent({
  name: 'NotifyButton',
  data() {
    return {
      date: '',
      homeData: [{}],
      notifications,
      alertButtons: ['OK'],
      isOpen: false,
    };
  },
  props: {
    holiday: {},
  },
  methods: {
    scheduleNotification(name: any, date: any) {
      try {
        const momentizedDate = moment(date, 'MMMM D');

        momentizedDate.set({
          hour: 7,
          minute: 0,
          second: 0,
        });

        if (momentizedDate.isBefore()) {
          momentizedDate.add(1, 'y');
        }

        const id = new Date().getTime() / 1000;

        this.isOpen = true;

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
      } catch {
        console.log('notification error');
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
