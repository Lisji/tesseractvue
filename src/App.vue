<template>
  <div class="tesseractor">
    <div class="input">
      <div class="input__img">
        <img id="img-text" class="image">
      </div>
      <input type="file" @change="onFileSelected">
    </div>
    <div class="trans">
      <button @click="recognize">轉換成文字</button>
    </div>
    <div class="output">
      <textarea class="output__text" v-model="output"/>
    </div>
  </div>
</template>

<script>
import { createWorker } from 'tesseract.js';
const worker = createWorker();

export default {
  name: 'app',
  data(){
    return {
      output: '' 
    }
  },
  methods: {
    async recognize() {
      const img = document.getElementById('img-text');
      await worker.load();
      await worker.loadLanguage('eng');
      await worker.initialize('eng');
      const { data } = await worker.recognize(img);
      this.output = data.text
    },
    onFileSelected(event) {
      var selectedFile = event.target.files[0];
      var reader = new FileReader();
      var imgTag = document.getElementById("img-text");
      imgTag.title = selectedFile.name;
      reader.onload = function(event) {
        imgTag.src = event.target.result;
      };
      reader.readAsDataURL(selectedFile);
    }
  }
}
</script>

<style lang="scss">
.tesseractor {
  display: flex;

  .input {
    &__img {
      display: flex;
      justify-content: center;
      align-items: center;
      border: 1px solid black;
      width: 400px;
      min-height: 350px;
      margin-bottom: 20px;

      .image {
        width: 100%;
      }
    }
  }

  .trans {
    display: flex;
    justify-content: center;
    align-items: center;
    margin: 0 10px;
  }

  .output {
    &__text {
      width: 400px;
      height: 350px;
    }
  }
}
</style>