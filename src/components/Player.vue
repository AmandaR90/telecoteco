<template>
  <v-row align="center" justify="center">
    <v-col style="text-align: right;"
    v-if="windowWidth > 500">
      <inline-svg
        width="35"
        height="35"
        :class="currentNote === 52 ? 'enabledNote' : 'disabledNote'"
        :src="require(`../../public/assets/icons/ta.svg`)"
      />
      <inline-svg
        width="35"
        height="35"
        :class="currentNote === 60 ? 'enabledNote' : 'disabledNote'"
        :src="require(`../../public/assets/icons/ti.svg`)"
      />
      <inline-svg
        width="30"
        height="30"
        :class="currentNote === 53 ? 'enabledNote' : 'disabledNote'"
        :src="require(`../../public/assets/icons/tu.svg`)"
      />
    </v-col>
    <v-col md="auto" style="text-align: center;">
      <v-btn
        color="success"
        fab
        depressed
        small
        :disabled="!selectedExercise"
        @click="playbackEngine.play()"
        v-if="playbackEngine.state !== 'PLAYING'"
      >
        <v-icon>mdi-play</v-icon>
      </v-btn>
      <v-btn
        color="error"
        fab
        depressed
        small
        :disabled="!selectedExercise"
        @click="playbackEngine.stop()"
        v-if="playbackEngine.state === 'PLAYING'"
      >
        <v-icon>mdi-stop</v-icon>
      </v-btn>
      <button v-shortkey="['space']" @shortkey="start_or_stop()"></button>
    </v-col>

    <v-col style="font-size: 14px">
      <!--<strong>BPM</strong>-->
      <v-btn icon small color="primary" @click="decreaseBpm()" :disabled="!selectedExercise">
        <v-icon>mdi-minus</v-icon>
      </v-btn>
      <button v-shortkey="['arrowdown']" @shortkey="decreaseBpm()"></button>
      {{playbackEngine.playbackSettings.bpm}}
      <v-btn icon small color="primary" @click="increaseBpm()" :disabled="!selectedExercise">
        <v-icon>mdi-plus</v-icon>
      </v-btn>
      <button v-shortkey="['arrowup']" @shortkey="increaseBpm()"></button>
    </v-col>
  </v-row>
  
</template>

<script>
import { mapMutations, mapGetters } from "vuex";

export default {
  computed: {
    ...mapGetters(['playbackEngine', 'currentNote', 'selectedExercise'])
  },
  methods: {
    ...mapMutations(['increaseBpm', 'decreaseBpm']),
    start_or_stop() {
      if (this.selectedExercise) {
        if (this.playbackEngine.state === 'PLAYING'){
          this.playbackEngine.stop();
        } else {
          this.playbackEngine.play();
        }
      }
    },
    onResize() {
      this.windowWidth = window.innerWidth
    }
  },
  data() {
    return {
      windowWidth: window.innerWidth,
      txt: ''
    }
  },

  watch: {
    windowWidth(newWidth, oldWidth) {
     this.txt = `it changed to ${newWidth} from ${oldWidth}`;
    }
  },

  mounted() {
    this.$nextTick(() => {
      window.addEventListener('resize', this.onResize);
    })
  },

  beforeDestroy() { 
    window.removeEventListener('resize', this.onResize); 
  },
};
</script> 

<style scoped>
.enabledNote {
  opacity: 0.8;
}
.disabledNote {
  opacity: 0.2;
}
</style>
