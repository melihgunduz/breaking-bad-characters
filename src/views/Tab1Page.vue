<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>Rastgele Karakter</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content v-if="durum.yukleniyor">
      <div class="loading-center">
        <ion-spinner name="dots" color="primary"></ion-spinner>
      </div>
    </ion-content>
    <ion-content :fullscreen="true" v-else>
      <ion-refresher slot="fixed" @ionRefresh="yenidenYukle">
        <ion-refresher-content></ion-refresher-content>
      </ion-refresher>
      <ion-card>
        <ion-img :src="durum.rastgeleKarakter.img"></ion-img>
        <ion-card-header>
          <ion-card-title>
            {{ durum.rastgeleKarakter.name }}
          </ion-card-title>
          <ion-card-subtitle>
            Takma ad: {{ durum.rastgeleKarakter.nickname }}</ion-card-subtitle
          >
        </ion-card-header>
      </ion-card>
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import axios from "axios";
import { reactive } from "vue";
import { defineComponent } from "vue";
import {
  IonPage,
  IonHeader,
  IonToolbar,
  IonTitle,
  IonContent,
  IonSpinner,
  IonRefresher,
  IonCard,
  IonCardTitle,
  IonCardSubtitle,
  IonCardHeader,
  IonImg,
  IonRefresherContent,
} from "@ionic/vue";

export default defineComponent({
  name: "Tab1Page",
  components: {
    IonHeader,
    IonToolbar,
    IonTitle,
    IonContent,
    IonPage,
    IonSpinner,
    IonRefresher,
    IonCard,
    IonCardTitle,
    IonCardSubtitle,
    IonCardHeader,
    IonImg,
    IonRefresherContent,
  },
  setup() {
    const durum = reactive({
      rastgeleKarakter: {},
      yukleniyor: false,
    });

    const rastgeleKarakterGetir = async (yuklenmeDurum: boolean) => {
      if (yuklenmeDurum) {
        durum.yukleniyor = true;
      }
      const res = await axios.get(
        "https://www.breakingbadapi.com/api/character/random"
      );

      if (res.data) {
        console.log(res.data[0]);
        durum.rastgeleKarakter = res.data[0];
      }
      durum.yukleniyor = false;
    };

    const yenidenYukle = (event: CustomEvent) => {
      rastgeleKarakterGetir(false);

      //eslint-disable-next-line
      //@ts-ignore
      event.target?.complete();
    };
    rastgeleKarakterGetir(true);

    return { durum, rastgeleKarakterGetir, yenidenYukle };
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