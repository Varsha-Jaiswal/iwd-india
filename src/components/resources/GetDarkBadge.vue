<template>
  <v-container id="dark" class="text-center">
    <v-row>
      <v-col md6 lg6 xs12 class="google-font" style="font-size:130%;">Dark Theme</v-col>
    </v-row>
    <v-row class="pa-0">
      <canvas id="canvas1"></canvas>
      <div style="display:none;">
        <img
          id="source1"
          contain
          src="@/assets/img/dark attendee filter.png"
          width="500px"
          height="500px"
        />
      </div>
      <div style="width:100%" v-if="!image">
        <p
          class="pt-3"
          style="font-size: 12px;color: rgba(0, 0, 0, 0.87);"
        >Please provide an image in which you are in the center.</p>
      </div>
    </v-row>
    <br />
    <v-row>
      <v-col>
        <v-btn
          color="#4285F4"
          outlined
          @click="uploadImage()"
          class="ma-0 google-font elevation-0 mr-2"
          style="text-transform: capitalize;border-radius: 5px;color: #4285F4; font-size:15px; padding: 7px;"
        >{{ image ? 'Upload Another' : 'Upload'}}</v-btn>
        <input type="file" accept="image/*" @change="upload($event)" hidden />
        <v-btn
          v-if="image"
          @click="download()"
          outlined
          color="#00BFA5"
          class="ma-0 google-font elevation-0"
          style="text-transform: capitalize;border-radius: 5px;color: #00BFA5;font-size:15px; padding: 7px;"
        >Download</v-btn>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  data() {
    return {
      image: "",
      shape: "original"
    };
  },
  mounted() {
    this.canvas = document.getElementById("canvas1");
    this.ctx = this.canvas.getContext("2d");
    this.draw();
  },
  methods: {
    download() {
      const a = document.createElement("a");
      const url = this.canvas.toDataURL("image/png;base64");
      a.download = "IWD-India-Badge Dark-Filter.png";
      a.href = url;
      a.click();
    },
    draw() {
      if (this.image) {
        this.canvas.width = 500;
        this.canvas.height = 500;
        const hRatio = this.canvas.width / this.image.width;
        const vRatio = this.canvas.height / this.image.height;
        const ratio = Math.max(hRatio, vRatio);
        const x = (this.canvas.width - this.image.width * ratio) / 2;
        const y = (this.canvas.height - this.image.height * ratio) / 2;
        this.ctx.drawImage(
          this.image,
          0,
          0,
          this.image.width,
          this.image.height,
          x,
          y,
          this.image.width * ratio,
          this.image.height * ratio
        );
        const image = document.getElementById("source1");
        this.ctx.drawImage(image, 0, 0, image.width, image.height);
      } else {
        this.ctx.canvas.width = 500;
        this.ctx.canvas.height = 500;
        this.ctx.fillStyle = "#e0e0e0";
        this.ctx.fillRect(0, 0, this.canvas.width, this.canvas.height);
        const image = document.getElementById("source1");
        image.addEventListener("load", () => {
          this.ctx.drawImage(image, 0, 0, image.width, image.height);
        });
      }
    },
    upload(e) {
      if (e && e.target.files && e.target.files[0]) {
        const reader = new FileReader();
        reader.onload = event => {
          const img = new Image();
          img.onload = () => {
            this.image = img;
            this.draw();
          };
          img.src = event.target.result;
        };
        reader.readAsDataURL(e.target.files[0]);
        this.draw();
      }
    },
    uploadImage() {
      document.querySelector("input").click();
    }
  }
};
</script>

<style scoped>
canvas {
  margin-left: auto;
  margin-right: auto;
  width: 350px;
  height: 350px;
}
@media only screen and (max-width: 1024px) {
  canvas {
    margin-left: auto;
    margin-right: auto;
    width: 300px;
    height: 300px;
  }
}
@media only screen and (max-width: 400px) {
  canvas {
    margin-left: auto;
    margin-right: auto;
    width: 250px;
    height: 250px;
  }
}
</style>
