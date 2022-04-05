<template>
  <div>
    <v-card class="mx-auto mt-3 " width="1150" height="500" id="f1">
      <h1 class="text-center pa-5">Nvision SDK: Object Detection Example</h1>
      <v-row justify="center">
        <v-col>
          <v-row class="mx-5 mt-5">
            <v-file-input
              v-model="file"
              color="deep-purple accent-4"
              counter
              label="File input"
              placeholder="Select your files"
              outlined
              :show-size="1000"
              class="px-5 py-5"
            >
            </v-file-input>
            <img v-bind:src="file" />
          </v-row>
          <v-row justify="end" class="mx-10 my-2">
            <v-btn
              v-if="file.length || file"
              v-on:click="() => request()"
              color="success"
              rounded
              x-large
              dark
              >Upload</v-btn
            >
            <!-- <v-img :src="url" /> -->
            <!-- <v-btn
              v-if="file.length || file"
              v-on:click="() => test()"
              color="success"
              rounded
              x-large
              dark
              >1</v-btn
            > -->
          </v-row>
          <v-col class="mx-10 my-2">
            <h2>Object Detection Result</h2>
            <br />
            <h3>Name : {{ dataobj[0] }}</h3>
            <h3>Parent : {{ dataobj[1] }}</h3>
            <h3>Confidence :{{ dataobj[2] }}</h3>
          </v-col>
          <v-row v-if="result !== ''">
            <v-card width="500" class="mx-10 my-2">
              <pre v-highlightjs="result"><code class="json"></code></pre>
            </v-card>
          </v-row>
        </v-col>
      </v-row>
    </v-card>
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
  data: {
    imageData: "", // we will store base64 format of image in this string
  },
  methods: {
    // previewImage: function (event) {
    //   var input = event.target;
    //   if (input.files && input.files[0]) {
    //     var reader = new FileReader();
    //     reader.onload = (e) => {
    //       this.imageData = e.target.result;
    //     };
    //     reader.readAsDataURL(input.files[0]);
    //   }
    // },
  },
})
export class NvisionRestfulExample extends Vue {
  public objectDetectionResult: any = {};
  public file?: File[] | File = [];
  public result = "";
  public dataobj = [];
  public conf = 0;

  private objectDetectionService = nvision.objectDetection({
    apiKey:
      "cdb29f355cb4059995e05420dc8d963f657898bf3a5f2f5e7a88c58279f5e4a0a1c4c4cf874594b42e413fc45c425425ac",
  });

  constructor() {
    super();
  }

  public request() {
    this.result = "";

    const reader = new FileReader();
    reader.onload = (event) => {
      this.objectDetectionService
        .predict({
          rawData: (reader.result as string).replace(/data:.+;base64,/, ""),
        })
        .then((data) => (this.result = JSON.stringify(data, null, 4)))
        .then(() => {
          const datajson = JSON.parse(this.result);
          this.dataobj[0] = datajson.detected_objects[0].name;
          this.dataobj[1] = datajson.detected_objects[0].parent;
          this.dataobj[2] = datajson.detected_objects[0].confidence;
          console.log(datajson.detected_objects[0].name);
          console.log(this.dataobj[2]);
        });
    };
    reader.readAsDataURL(this.file as File);
  }
}

export default NvisionRestfulExample;
</script>
<style scoped>
#f1{
  border: 2px solid #220ccb;
}
</style>