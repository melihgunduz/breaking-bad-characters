<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>Karakter Listesi</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content v-if="durum.yukleniyor">
      <div class="loading-center">
        <ion-spinner name="dots" color="danger"></ion-spinner>
      </div>
    </ion-content>
    <ion-content :fullscreen="true" v-else>
      <ion-list>
        <ion-item
          v-for="karakter in durum.karakterListesi"
          :key="karakter.char_id"
          @click="() => router.push(`/karakter/${karakter.char_id}`)"
        >
          <ion-avatar slot="start">
            <ion-img :src="karakter.img"></ion-img>
          </ion-avatar>
          <ion-label>
            <h2>{{ karakter.name }}</h2>
          </ion-label>
        </ion-item>
      </ion-list>
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import { reactive } from "vue";
import axios from "axios";
import { useRouter } from "vue-router";
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
  IonItem,
  IonLabel,
  IonList,
  IonAvatar,
  IonRefresherContent,
} from "@ionic/vue";

export default defineComponent({
  name: "Tab2Page",
  components: {
    IonHeader,
    IonToolbar,
    IonTitle,
    IonContent,
    IonPage,
    IonSpinner,
    // IonRefresher,
    // IonCard,
    // IonCardTitle,
    // IonCardSubtitle,
    // IonCardHeader,
    // IonRefresherContent,
    IonImg,
    IonItem,
    IonLabel,
    IonList,
    IonAvatar,
  },
  setup() {
    const router = useRouter();
    const durum = reactive({
      yukleniyor: false,
      karakterListesi: {},
    });
    const karakterlerGetir = async () => {
      durum.yukleniyor = true;
      const res = await axios.get(
        "https://www.breakingbadapi.com/api/characters"
      );
      if (res.data) {
        durum.karakterListesi = res.data;
      }
      durum.yukleniyor = false;
    };
    karakterlerGetir();
    return { durum, router, karakterlerGetir };
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