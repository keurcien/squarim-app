<template>
  <div>
    <div class="header">
      <h1>squarim</h1>
    </div>
    <div class="dropzone-container">
      <UploadImages
        @changed="handleImages"
        uploadMsg="Déposer les images que vous souhaitez traiter"
        clearAll="Retirer toutes les images"
        ref="dropzone"
      />
      <div class="input-container" v-if="images.length > 0">
        <span>Largeur à gauche</span>
        <input v-model="left" />
        <span>Largeur à droite</span>
        <input v-model="right" />
        <span>Miroir</span>
        <input type="checkbox" id="checkbox" v-model="mirror" />
        <button id="process-btn" @click="process">Lancer le traitement</button>
      </div>
      <div class="loader" v-if="loading">
        <LoadingSpinner :size="75" style="margin-bottom: 40px; margin: auto" />
      </div>
      <div
        v-if="transformedImages.length > 0 && !loading"
        class="preview-container"
      >
        <div class="preview-images-container">
          <img
            class="processed-image"
            v-for="(image, index) in transformedImages"
            :key="index"
            :src="image.dataURL"
          />
        </div>
        <button id="download-btn" @click="download">Télécharger</button>
      </div>
    </div>
  </div>
</template>

<script>
import UploadImages from "vue-upload-drop-images";
import { Circle as LoadingSpinner } from "vue-loading-spinner";

export default {
  components: {
    UploadImages,
    LoadingSpinner,
  },
  data() {
    return {
      left: 50,
      right: 50,
      mirror: false,
      images: [],
      transformedImages: [],
      loading: false,
    };
  },
  methods: {
    handleImages(files) {
      this.images = [];
      this.transformedImages = [];
      for (let i = 0; i < files.length; i++) {
        let reader = new FileReader();
        reader.readAsDataURL(files[i]);
        reader.onload = () => {
          this.images.push({
            name: files[i].name,
            dataURL: reader.result,
          });
        };
      }
    },
    async process() {
      this.loading = true;
      this.transformedImages = [];
      for (let i = 0; i < this.images.length; i++) {
        let data = new FormData();
        data.append("name", this.images[i].name);
        data.append("left", this.left);
        data.append("right", this.right);
        data.append("file", this.images[i].dataURL);
        data.append("mirror", this.mirror);
        const url = "https://squarim-f5ljwmnzga-ew.a.run.app/";
        //const url = "http://127.0.0.1:8000/";
        const response = await fetch(url, {
          method: "POST",
          body: data,
        });
        const file = await response.json();
        this.transformedImages.push(file);
      }
      this.loading = false;
    },
    download() {
      for (let i = 0; i < this.transformedImages.length; i++) {
        let link = document.createElement("a");
        link.download = this.transformedImages[i].name;
        link.href = this.transformedImages[i].dataURL;
        link.click();
      }
    },
  },
};
</script>

<style>
.header {
  width: 100%;
  opacity: 0.9;
  padding-left: 50px;
  padding-right: 50px;
  margin-bottom: 10px;
}

.header > h1 {
  color: #1005a0;
}

.dropzone-container {
  width: 90%;
  padding-left: 50px;
  padding-right: 50px;
}

.imageHolder {
  width: unset !important;
  height: 200px;
}

.plus {
  line-height: 30px !important;
}

.delete {
  border: 1px solid red;
}

.delete:hover {
  color: red !important;
  background: white !important;
  border: 1px solid red;
}

.input-container {
  display: flex;
  align-items: center;
  justify-content: center;
}

.input-container > span {
  margin-left: 10px;
}

.input-container > input {
  width: 40px;
  text-align: right;
  margin: 5px;
}

.loader {
  text-align: center;
  padding: 75px;
}

.processed-image {
  max-width: 200px;
  object-fit: scale-down;
}

.clearButton:hover {
  color: red;
  cursor: pointer;
}

#process-btn {
  border-radius: 25px;
  border: 1px solid #1005a0;
  padding-left: 20px;
  padding-right: 20px;
  padding-top: 10px;
  padding-bottom: 10px;
  background: #1005a0;
  color: white;
  font-weight: 600;
  font-family: Roboto;
  margin: 10px;
}

#process-btn:hover {
  background: white;
  color: #1005a0;
  font-weight: 600;
  cursor: pointer;
}

#download-btn {
  border-radius: 25px;
  border: transparent;
  padding-left: 20px;
  padding-right: 20px;
  padding-top: 10px;
  padding-bottom: 10px;
  background: #ff6c62;
  color: white;
  font-weight: 600;
  font-family: Roboto;
  margin: 10px;
}

#download-btn:hover {
  background: white;
  border: 1px solid #ff6c62;
  color: #ff6c62;
  font-weight: 600;
  cursor: pointer;
}

.preview-container {
  text-align: center;
}

.preview-images-container {
  display: flex;
  width: 100%;
  position: relative;
  border-radius: 10px;
  border: 0.5px solid #a3a8b1;
  padding: 30px;
  background: #f7fafc;
}

.preview-images-container > img {
  margin: 10px;
}
</style>