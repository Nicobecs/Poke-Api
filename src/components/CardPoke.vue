<script setup>
  import { ref, computed} from 'vue';

  const props = defineProps({
    namePokemon: {
      type: String,
      required: true,
    },
    imageUrl: {
      type: String,
      required: true,
    },
  });

  const emit = defineEmits(['send-name']);

  const inputUser = ref('');
  const correct = ref(false);

  const sendName = () => {
    if (inputUser.value.toLowerCase().trim() === props.namePokemon.toLowerCase()) {
      correct.value = true;
      emit('send-name', props.namePokemon);
    } else {
      alert('Siga intentando');
    }
  }
</script>
<template>
  <div class="card col-3 border border-0 my-3">
    <img :class="[!correct ? 'filter' : '', 'pokemon-image', 'mx-auto']" :src="imageUrl" :alt="namePokemon" />
    <div v-if="correct">
      <p class="name">{{ namePokemon }}</p>
    </div>
    <form v-else @submit.prevent="sendName">
      <input class="col-12 form-control mb-2" v-model="inputUser" />
      <button class="col-12 btn btn-light">Descubrir</button>
      <p class="incorrect" v-show="correct == false">Incorrecto, intentalo de nuevo</p>
    </form>
  </div>
</template>
<style scoped>
  .pokemon-image{
    width: 10rem;
    height: 10rem;
  }
  .filter {
    filter: blur(5px) grayscale(100%);
  }
</style>
