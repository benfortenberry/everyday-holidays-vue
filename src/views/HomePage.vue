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

								<ion-card-content>
									{{ h.blurb }}
								</ion-card-content>
							</ion-card>
						</ion-col>
					</ion-row>
				</ion-grid>
			</ion-content>
		</ion-content>
	</ion-page>
</template>

<script  lang="ts">
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
} from "@ionic/vue";
import { IonPage } from "@ionic/vue";
import { defineComponent } from "vue";
import moment from "moment";
import HolidayData from "../../public/assets/json/holidays.json";
import { arrowForwardSharp, arrowBackSharp } from "ionicons/icons";
export default defineComponent({
	name: "HomePage",
	data() {
		return {
			date: "",
			HolidayData,
			homeData: [{}],
			arrowForwardSharp,
			arrowBackSharp,
			plusMinus: 0,
		};
	},
	ionViewDidEnter() {
		this.date = moment().format("MMMM D");
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
			this.date = moment().add(this.plusMinus, "d").format("MMMM D");
			this.getHomeData();
		},
		addToDate() {
			this.plusMinus++;
			this.date = moment().add(this.plusMinus, "d").format("MMMM D");
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
	},
});
</script>
