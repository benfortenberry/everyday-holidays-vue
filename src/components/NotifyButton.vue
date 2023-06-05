<template>
  <ion-button
    @click="scheduleNotification(holiday.name, holiday.date)"
    fill="clear"
    ><ion-icon :icon="notifications"></ion-icon
  ></ion-button>

  <ion-alert
    :is-open="isOpen"
    header="Success"
    message="A reminder will be sent every year."
    :buttons="alertButtons"
    @didDismiss="isOpen = false"
  ></ion-alert>
</template>

<script lang="ts">
import { IonButton, IonIcon, IonAlert } from '@ionic/vue';
import { defineComponent } from 'vue';
import { notifications } from 'ionicons/icons';
import { LocalNotifications } from '@capacitor/local-notifications';

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
                at: new Date(Date.now() + 1000 * 3),
                repeats: true,
                count: 99,
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