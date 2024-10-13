<template>
  <q-page class="flex flex-center row" style="max-width: 640px; margin:0 auto">
    <audio id="audioPlayer" src="musique-fort-boyard.mp3" type="audio/mpeg"></audio>
    <div class="column">
      <div class="col">
        <img class="image" :class="{ 'image-scale': result && result === 'ok' }" alt="Quasar logo"
          src="~assets/logo.png" width="600">
        <q-card v-if="result && result === 'ok'" class="my-card bg-secondary text-white">
          <q-card-section>
            <h4 style="margin: 0">😃🎉 Bravo !! voici l'indice :</h4>
            <h1 style="margin: 0">{{ indice }}</h1>
          </q-card-section>
        </q-card>
        <q-card v-if="result && result === 'ko'" class="my-card bg-red text-white">
          <q-card-section>
            😱 Code incorrect
          </q-card-section>
        </q-card>
      </div>
      <q-form @submit="onSubmit">
        <div class="col q-mt-lg">
          <q-input v-model="answer" filled label="Saisir le code" />
        </div>
        <div class="col flex flex-center q-mt-lg">
          <q-btn style="width:100%" type="submit" outline size="lg" label="Valider" />
        </div>
      </q-form>
    </div>


  </q-page>
</template>

<script setup>
import { useQuasar } from 'quasar'
import { ref } from 'vue'
const $q = useQuasar()
$q.dark.set(true)

window.addEventListener('load', function () {
  const audio = document.getElementById('audioPlayer');
  audio.play().catch(error => {
    console.log("Lecture automatique désactivée par le navigateur :", error);
  });
});

const answer = ref('')
const result = ref('')
const indice = ref('')

function onSubmit () {
  if (answer.value === '64781') {
    result.value = 'ok'
    indice.value = 'Medecin'
  } else if (answer.value === '9845') {
    result.value = 'ok'
    indice.value = 'Ambulance'
  } else if (answer.value === '4678') {
    result.value = 'ok'
    indice.value = 'Urgence'
  }
  else {
    result.value = 'ko'
  }
}


defineOptions({
  name: 'IndexPage'
});
</script>

<style>
.image {
  transition: all 10s;
  animation: fadeInOut 10s infinite;
}

.image-scale {
  animation: success 10s infinite;
}

@keyframes success {
  0% {
    transform: scale(1);
  }

  50% {
    transform: scale(40);
  }

  100% {
    transform: scale(1);
  }
}

@keyframes fadeInOut {
  0% {
    opacity: 1;
    /* Complètement visible */
  }

  50% {
    opacity: 0.1;
    /* Invisible */
  }

  100% {
    opacity: 1;
    /* Complètement visible */
  }
}
</style>
