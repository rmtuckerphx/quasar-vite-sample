<template>
  <q-page class="row items-center justify-evenly">
    <q-btn color="primary" @click="startClick">Start</q-btn>
  </q-page>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import MicrophoneStream from 'microphone-stream';

let micStream;

export default defineComponent({
  name: 'IndexPage',
  components: {},
  setup() {
    const startClick = async () => {
      console.log('start');
      micStream = new MicrophoneStream();

      const media = await navigator.mediaDevices.getUserMedia({
        video: false,
        audio: true
      });

      // TODO: this causes the Buffer error
      micStream.setStream(media);
    }
    return { startClick };
  }
});
</script>
