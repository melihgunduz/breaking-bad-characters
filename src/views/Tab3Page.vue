<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>Karakter Arama</ion-title>
      </ion-toolbar>
      <ion-toolbar>
        <ion-searchbar
          debounce="500"
          :onIonChange="(e) => sonucGetir(e.detail.value)"
        >
        </ion-searchbar>
      </ion-toolbar>
    </ion-header>
    <ion-content v-if="yukleniyor">
      <div class="center">
        <ion-spinner name="dots" color="secondary"></ion-spinner>
      </div>
    </ion-content>
    <ion-content :fullscreen="true" v-else>
      <div v-for="karakter in karakterler" :key="karakter.char_id">
        <ion-card @click="() => router.push(`/karakter/${karakter.char_id}`)">
          <ion-img :src="karakter.img"></ion-img>
          <ion-card-header>
            <ion-card-title>{{ karakter.name }}</ion-card-title>
          </ion-card-header>
          <ion-card-content></ion-card-content>
        </ion-card>
      </div>
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import {
  IonPage,
  IonHeader,
  IonToolbar,
  IonTitle,
  IonContent,
  IonSearchbar,
  IonSpinner,
  IonCard,
  IonCardHeader,
  IonImg,
  IonCardTitle,
  IonCardContent,
} from "@ionic/vue";
import { ref } from "vue";
import axios from "axios";
import { useRouter } from "vue-router";

export default defineComponent({
  name: "Tab3Page",
  components: {
    IonHeader,
    IonToolbar,
    IonTitle,
    IonContent,
    IonPage,
    IonSearchbar,
    IonSpinner,
    IonCard,
    IonCardHeader,
    IonImg,
    IonCardTitle,
    IonCardContent,
  },

  setup() {
    const router = useRouter();
    const yukleniyor = ref(false);
    const karakterler = ref([]);

    const sonucGetir = async (veri: string) => {
      console.log(veri);
      yukleniyor.value = true;

      if (veri) {
        const res = await axios.get(
          `https://www.breakingbadapi.com/api/characters?name=${veri}`
        );

        if (res.data) {
          console.log(res.data);
          karakterler.value = res.data;
          console.log(karakterler);
        }
        yukleniyor.value = false;
      }
    };

    return { sonucGetir, yukleniyor, karakterler, router };
  },
});
</script>

<style scoped>
.center {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 80vh;
}

ion-spinner {
  transform: scale(2);
}
</style>
