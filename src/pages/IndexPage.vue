<template>
  <q-page class="row items-center justify-evenly">
    <q-btn color="primary" @click="startClick">Start</q-btn>
    <q-btn color="secondary outline" @click="stopClick">Stop</q-btn>
  </q-page>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import MicrophoneStream from 'microphone-stream';
import { Readable } from 'stream';
import { AudioStream } from '@aws-sdk/client-transcribe-streaming';

let micStream: MicrophoneStream;

export default defineComponent({
  name: 'IndexPage',
  components: {},
  setup() {
    micStream = new MicrophoneStream();

    async function* createAudioStream(audio: Readable): AsyncIterable<AudioStream> {
      for await (const chunk of audio) {
        const audioChunk = { AudioEvent: { AudioChunk: chunk } };
        console.log('audioChunk', { audioChunk });
        yield audioChunk
      }
    }

    const startClick = async () => {
      console.log('start');

      try {
        const media = await navigator.mediaDevices.getUserMedia({
          video: false,
          audio: true
        });

        micStream.setStream(media);
        micStream.on('data', (chunk) => {
          console.log('data', { chunk })
        });

        createAudioStream(micStream);

      } catch (error) {
        console.log(error);
      } finally {
        if (micStream) {
          micStream.stop();
        }
      }
    }

    const stopClick = async () => {
      console.log('stop');
      if (micStream) {
        micStream.stop();
      }
    }

    return {
      startClick,
      stopClick
    };
  }
});
</script>
