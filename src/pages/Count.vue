<template>
  <q-page class="flex flex-center row" style="max-width: 640px; margin:0 auto">
    <audio id="audioPlayer" src="musique-fort-boyard.mp3" type="audio/mpeg"></audio>
    <div class="column">
      <div class="col">
        <img class="image" :class="{ 'image-scale': result && result === 'ok' }" alt="Quasar logo"
          src="~assets/logo.png" width="600">
      </div>
      <div class="col q-mt-lg">
        <Countdown />
      </div>
    </div>


  </q-page>
</template>

<script setup>
import { useQuasar } from 'quasar'
import Countdown from 'src/components/Count-down.vue';
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

function onSubmit () {
  if (answer.value === '1234') {
    result.value = 'ok'
    /* const audio = document.getElementById('audioPlayer');
    audio.play().catch(error => {
      console.log("Erreur lors de la lecture : ", error);
    }); */
  } else {
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
