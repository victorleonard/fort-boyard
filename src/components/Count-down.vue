<template>
  <div class="countdown-timer">
    <audio id="audioPlayer" src="musique-fort-boyard.mp3" type="audio/mpeg"></audio>
    <h1 id="countdown" style="margin-top: 0;">
      <span v-if="minutes > 0">{{ minutes }}m</span> {{ seconds }}s
    </h1>
    <div>
      <label for="timeInput">Définir le temps (en secondes) :</label>
      <q-input v-model="timeInput" type="number" filled label="secondes" id="timeInput" />
    </div>

    <q-btn @click="startCountdown" style="width:100%" outline label="Démarrer" />
    <q-btn @click="resetCountdown" style="width:100%" outline label="Réinitialiser" />
  </div>
</template>

<script>
import { ref, onBeforeUnmount } from 'vue';

export default {
  setup () {
    const timeInput = ref(60); // Valeur par défaut (60 secondes)
    const timeLeft = ref(0);
    const minutes = ref(0);
    const seconds = ref(0);
    let countdownInterval = null; // Référence de l'intervalle

    const startCountdown = () => {
      const audio = document.getElementById('audioPlayer');
      audio.currentTime = 0;
      audio.play().catch(error => {
        console.log("Erreur lors de la lecture : ", error);
      });
      if (countdownInterval) {
        clearInterval(countdownInterval); // Stoppe le compte à rebours s'il est déjà actif
      }

      timeLeft.value = timeInput.value; // Initialise le temps restant
      updateTimeDisplay();

      countdownInterval = setInterval(() => {
        if (timeLeft.value > 0) {
          timeLeft.value--;
          updateTimeDisplay();
        } else {
          audio.pause();
          clearInterval(countdownInterval); // Arrêter l'intervalle quand le compte à rebours est terminé
        }
      }, 1000);
    };

    const resetCountdown = () => {
      const audio = document.getElementById('audioPlayer');
      audio.pause();
      clearInterval(countdownInterval); // Arrête l'intervalle actif
      timeLeft.value = timeInput.value; // Réinitialise le temps
      updateTimeDisplay();
    };

    const updateTimeDisplay = () => {
      minutes.value = Math.floor(timeLeft.value / 60); // Calculer les minutes restantes
      seconds.value = timeLeft.value % 60; // Calculer les secondes restantes
    };

    onBeforeUnmount(() => {
      clearInterval(countdownInterval); // Nettoyer l'intervalle avant de détruire le composant
    });

    return {
      timeInput,
      minutes,
      seconds,
      startCountdown,
      resetCountdown,
    };
  },
};
</script>

<style scoped>
.countdown-timer {
  text-align: center;
  margin-top: 20px;
}

button {
  margin: 10px;
  padding: 10px;
  font-size: 16px;
}

input {
  margin: 10px;
  padding: 5px;
  font-size: 16px;
}
</style>
