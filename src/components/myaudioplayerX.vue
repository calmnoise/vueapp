<template>
  <vuetify-audio
    class="my-10"
    :file="`./train/${this.tc.train}/${this.tc.stations}/${this.tc.platform}.mp3`"
    :ended="audioFinish"
  ></vuetify-audio>
</template>

<script>
import VuetifyAudio from "vuetify-audio";
export default {
  data() {
    return {
      tc: {
        key: 12342,
        timing: "8:00",
        train: "0081",
        stations: "j",
        platform: "1"
      },
      file: null,
      msgs: ["No message now."]
    };
  },
  computed: function() {
    file = `./train/${this.tc.train}/${this.tc.stations}/1.mp3`;
  },
  components: {
    "vuetify-audio": VuetifyAudio
  },
  methods: {
    audioFinish() {
      this.msgs.push("The audio finished.");
    }
  }
};
</script>