<template>
  <div>
    <div>
      <v-col>
        <v-card width="1150" id="f1">
          <v-row justify="center">
            <h1 class="text-center mt-5">
              Nvision SDK: Object Detection Example
            </h1>
            <v-col cols="8" align-self="center" class="ml-auto">
              <h2 class="text-center my-3">Camera</h2>
            </v-col>
            <v-col cols="4"> </v-col>

            <v-row justify="center" class="mx-8">
              <v-col>
                <v-row>
                  <v-card
                    width="670"
                    height="500"
                    flat
                    id="frame"
                    class="my-2 p-1"
                  >
                    <div class="camera" width="680" height="800">
                      <video
                        id="video"
                        :height="height"
                        :width="width"
                        class="mx-3 my-2"
                      >
                        Video stream not available.
                      </video>
                    </div>
                  </v-card>
                </v-row>
                <v-row class="mx-auto my-5">
                  <v-spacer></v-spacer>

                  <v-col cols="4">
                    <v-btn v-on:click="startCamera" color="success" block
                      >Start camera</v-btn
                    >
                  </v-col>
                  <v-spacer></v-spacer>
                  <v-col cols="4" align-self="center">
                    <v-btn id="snap" color="error" block>Capture</v-btn>
                  </v-col>
                  <v-spacer></v-spacer>
                  <!-- 
                  <v-col cols="4" align-self="center">
                    <v-btn v-on:click="startCamera" color="primary" block
                      >Upload</v-btn
                    >
                  </v-col>
                  <v-spacer></v-spacer> -->
                </v-row>
              </v-col>

              <v-col class="mx-12 my-7">
                <h2 class="my-3">Picture</h2>
                <v-card height="300" width="310">
                  <canvas
                    id="canvas"
                    :height="this.height"
                    :width="this.width"
                  ></canvas>
                </v-card>
                <v-btn
                  v-on:click="startCamera"
                  color="primary"
                  block
                  class="my-4"
                  >Upload</v-btn
                >
              </v-col>

              <!-- <v-col align-self="center">
            <canvas id="canvas" :height="100" :width="100"></canvas>
          </v-col> -->
            </v-row>
          </v-row>
        </v-card>
        <v-col></v-col>
      </v-col>

      <!-- <v-card class="my-5" height="500">
        <canvas id="canvas" :height="this.height" :width="this.width"></canvas>
      </v-card> -->
    </div>
  </div>
</template>

<script lang="ts">
import HelloWorld from "../components/HelloWorld.vue";
import Component from "vue-class-component";
import Vue from "vue";
import nvision from "@nipacloud/nvision";

@Component({
  components: {
    HelloWorld,
  },
})
export class NvisionWebSocketExample extends Vue {
  public file?: File[] | File = [];
  public result = "";

  public streaming = false;
  public height = 0;
  public width = 640;

  private objectDetectionService = nvision
    .objectDetection({
      streamingKey:
        "cdb29f355cb4059995e05420dc8d963f657898bf3a5f2f5e7a88c58279f5e4a0a1c4c4cf874594b42e413fc45c425425ac",
    })
    .stream();

  constructor() {
    super();
  }

  public async startCamera() {
    const video: HTMLVideoElement = document.getElementById(
      "video"
    ) as HTMLVideoElement;
    const canvas: HTMLCanvasElement = document.getElementById(
      "canvas"
    ) as HTMLCanvasElement;
    const snap = document.getElementById("snap");

    await navigator.mediaDevices
      .getUserMedia({ video: true, audio: false })
      .then((stream) => {
        video.srcObject = stream;
        video.play();
      })
      .catch((err) => {
        console.log("An error occurred: " + err);
      });

    video.addEventListener(
      "canplay",
      async (ev: any) => {
        if (!this.streaming) {
          this.height = video.videoHeight / (video.videoWidth / this.width);

          if (isNaN(this.height)) {
            this.height = this.width / (4 / 3);
          }
          this.streaming = true;

          await this.objectDetectionService.connect();

          setInterval(() => {
            const context = canvas.getContext("2d");
            context!.drawImage(video, 0, 0, this.width, this.height);
            canvas.toBlob((blob: any) => {
              blob.arrayBuffer().then((buffer: any) => {
                const reader = new FileReader();
                reader.onload = () => {
                  this.objectDetectionService.predict({
                    rawData: new Uint8Array(buffer),
                  });
                };
                reader.readAsDataURL(blob);
              });
            }, "image/jpeg");
          }, 20000);
        }
      },
      false
    );
    const context = canvas.getContext("2d");
    snap.addEventListener("click", () => {
      context!.drawImage(video, 0, 0, 300, 300);
    });
  }
}

export default NvisionWebSocketExample;
</script>
<style scoped>
#frame {
  border: 3px dashed #220ccb;
  background-image: url("https://i.pinimg.com/564x/26/74/84/2674845ed281cf90e9cce3eba31d5ca4.jpg");
  background-blend-mode: hard-light;
  background-position: center;
  background-size: 30%;
}
#f1{
  border: 2px solid #220ccb;
}
</style>
