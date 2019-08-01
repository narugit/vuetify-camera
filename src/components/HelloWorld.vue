<template>
  <v-container>
    <v-layout text-center wrap>
      <v-flex mb-4>
        <h1 class="display-2 font-weight-bold mb-3">Camera</h1>
      </v-flex>
    </v-layout>
    <div id="app">
      <div>
        <video ref="video" id="video" width="640" height="480" autoplay></video>
      </div>
      <div>
        <button id="snap" v-on:click="capture()">Snap Photo</button>
      </div>
      <canvas ref="canvas" id="canvas" width="640" height="480"></canvas>
      <ul>
        <v-list v-for="c in captures" :key="c">
          <img v-bind:src="c" height="50" />
        </v-list>
      </ul>
    </div>
  </v-container>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      video: {},
      canvas: {},
      captures: []
    };
  },
  mounted() {
    this.video = this.$refs.video;
    if (navigator.mediaDevices && navigator.mediaDevices.getUserMedia) {
      navigator.mediaDevices.getUserMedia({ video: true }).then(stream => {
        this.video.srcObject = stream;
        this.video.play();
      });
    }
  },
  methods: {
    capture() {
      this.canvas = this.$refs.canvas;
      var ctx = this.canvas.getContext("2d");

      ctx.drawImage(this.video, 0, 0, 640, 480);
      this.captures.push(this.$refs.canvas.toDataURL("image/png"));
    }
  }
};
</script>

<style>
#app {
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
#video {
  background-color: #000000;
}
#canvas {
  display: none;
}
li {
  display: inline;
  padding: 5px;
}
</style>
