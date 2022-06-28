<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-buttons>
          <ion-back-button color="danger"></ion-back-button>
        </ion-buttons>
        <ion-title>{{ durum.karakter.name }}</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content v-if="durum.yukleniyor">
      <div class="loading-center">
        <ion-spinner name="dots" color="danger"></ion-spinner>
      </div>
    </ion-content>
    <ion-content :fullscreen="true" v-else>
      <ion-card>
        <ion-img :src="durum.karakter.img"> </ion-img>
        <ion-card-header>
          <ion-card-title>
            {{ durum.karakter.name }}
          </ion-card-title>
          <ion-card-subtitle
            >Takma ad: {{ durum.karakter.nickname }}</ion-card-subtitle
          >
        </ion-card-header>
        <ion-card-content>
          <ion-list>
            <ion-list-header>Meslekleri</ion-list-header>
            <ion-item
              v-for="meslek in durum.karakter.occupation"
              :key="meslek"
              >{{ meslek }}</ion-item
            >
          </ion-list>
        </ion-card-content>
      </ion-card>
    </ion-content>
  </ion-page>
</template>

<script>
import { useRoute } from "vue-router";
import { defineComponent } from "vue";
import {
  IonPage,
  IonHeader,
  IonToolbar,
  IonTitle,
  IonContent,
  IonSpinner,
  IonList,
  IonItem,
  IonImg,
  IonButtons,
  IonBackButton,
  IonCard,
  IonCardTitle,
  IonCardSubtitle,
  IonCardHeader,
  IonCardContent,
  IonListHeader,
} from "@ionic/vue";
import { reactive } from "vue";
import axios from "axios";

export default defineComponent({
  components: {
    IonPage,
    IonToolbar,
    IonTitle,
    IonContent,
    IonSpinner,
    IonList,
    IonItem,
    IonImg,
    IonHeader,
    IonButtons,
    IonBackButton,
    IonCard,
    IonCardTitle,
    IonCardSubtitle,
    IonCardHeader,
    IonCardContent,
    IonListHeader,
  },

  setup() {
    const route = useRoute();
    console.log(route.params.id);

    const karakterId = route.params.id;

    const durum = reactive({
      yukleniyor: false,
      karakter: {},
    });

    const karakterGetir = async (id) => {
      durum.yukleniyor = true;
      const res = await axios.get(
        `https://www.breakingbadapi.com/api/characters/${id}`
      );

      if (res.data) {
        durum.karakter = res.data[0];
      }

      durum.yukleniyor = false;
    };
    karakterGetir(karakterId);
    return { durum };
  },
});
</script>

<style scoped>
.loading-center {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 90vh;
}

ion-spinner {
  transform: scale(2);
}
</style>