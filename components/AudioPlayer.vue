
<template>
  <v-card id="audio-player-root" color="#002978" flat tile width="400px">
    <audio :id="playerid" ref="player" style="display: none">
      <source :src="url" type="audio/mpeg" />
    </audio>

    <div class="d-flex">
      <v-icon v-show="!isPlaying" color="white" x-large @click="toggleAudio()"
        >mdi-play</v-icon
      >
      <v-icon v-show="isPlaying" color="white" x-large @click="toggleAudio()"
        >mdi-pause</v-icon
      >

      <v-card-text class="mt-5 mx-5">
        <v-slider
          v-model="volume"
          max="1"
          step="0.1"
          dark
          x-large
          :prepend-icon="
            volume >= 0.5
              ? 'mdi-volume-high'
              : volume > 0
              ? 'mdi-volume-medium'
              : 'mdi-volume-off'
          "
          @input="updateVolume(volume)"
        ></v-slider>
      </v-card-text>
    </div>
  </v-card>
</template>

<script>
export default {
  props: ['url', 'playerid'],
  data() {
    return {
      playbackTime: 0,
      audioDuration: 100,
      audioLoaded: false,
      isPlaying: false,
      volume: 0.5,
      muted: false,
    }
  },
  mounted() {
    //  nextTick code will run only after the entire view has been rendered
    this.$nextTick(function () {
      const audio = this.$refs.player
      // Wait for audio to load, then run initSlider() to get audio duration and set the max value of our slider
      //  "loademetadata" Event https:// www.w3schools.com/tags/av_event_loadedmetadata.asp
      audio.addEventListener(
        'loadedmetadata',
        function (e) {
          this.initSlider()
        }.bind(this)
      )

      //  "canplay" HTML Event lets us know audio is ready for play https:// www.w3schools.com/tags/av_event_canplay.asp
      audio.addEventListener(
        'canplay',
        function (e) {
          this.audioLoaded = true
        }.bind(this)
      )

      // Wait for audio to begin play, then start playback listener function
      this.$watch('isPlaying', function () {
        if (this.isPlaying) {
          const audio = this.$refs.player
          this.initSlider()
          // console.log("Audio playback started.");

          // prevent starting multiple listeners at the same time
          if (!this.listenerActive) {
            this.listenerActive = true

            audio.addEventListener('freqtimeupdate', this.playbackListener)
          }
        }
      })

      // Update current audio position when user drags progress slider
      this.$watch('playbackTime', function () {
        // const audio = this.$refs.player

        const diff = Math.abs(this.playbackTime - this.$refs.player.currentTime)

        // Throttle synchronization to prevent infinite loop between playback listener and this watcher
        if (diff > 0.01) {
          this.$refs.player.currentTime = this.playbackTime
        }
      })
    })
  },
  methods: {
    updateVolume(volume) {
      const audio = this.$refs.player
      audio.volume = volume
    },
    // toggleMute() {
    //   const audio = this.$refs.player
    //   console.log(audio.mute)

    // this.$refs.player.mute(!this.muted)
    // this.muted = !this.muted
    // },
    initSlider() {
      const audio = this.$refs.player
      if (audio) {
        this.audioDuration = Math.round(audio.duration)
      }
    },

    // Playback listener function runs every 100ms while audio is playing
    playbackListener(e) {
      const audio = this.$refs.player

      // Sync local 'playbackTime' const to audio.currentTime and update global state
      this.playbackTime = audio.currentTime

      // console.log("update: " + audio.currentTime);

      // Add listeners for audio pause and audio end events
      audio.addEventListener('ended', this.endListener)
      audio.addEventListener('pause', this.pauseListener)
    },

    // Function to run when audio is paused by user
    pauseListener() {
      this.isPlaying = false
      this.listenerActive = false
      this.cleanupListeners()
    },
    // Function to run when audio play reaches the end of file
    endListener() {
      this.isPlaying = false
      this.listenerActive = false
      this.cleanupListeners()
    },
    // Remove listeners after audio play stops
    cleanupListeners() {
      const audio = this.$refs.player
      audio.removeEventListener('freqtimeupdate', this.playbackListener)
      audio.removeEventListener('ended', this.endListener)
      audio.removeEventListener('pause', this.pauseListener)

      // console.log("All cleaned up!");
    },
    toggleAudio() {
      const audio = this.$refs.player
      // const audio = document.getElementById("audio-player");
      if (audio.paused) {
        audio.play()
        this.isPlaying = true
      } else {
        audio.pause()
        this.isPlaying = false
      }
    },
  },
}
</script>

<style>
.mdi-volume-high {
  font-size: 35px !important;
}
.mdi-volume-medium {
  font-size: 35px !important;
}
.mdi-volume-off {
  font-size: 35px !important;
}
</style>