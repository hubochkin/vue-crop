<template>
  <div class="container">
    <div class="cropper-container">
      <button @click="triggerFileUpload">Upload Image</button>
      <input type="file" ref="fileInput" @change="onFileChange" style="display: none;" />
      
      <vue-cropper
        v-if="imageUrl"
        ref="cropper"
        :src="imageUrl"
        :aspect-ratio="16 / 9"
        :view-mode="1"
        :drag-mode="'move'"
        :guides="false"
        :center="true"
        :auto-crop-area="1"
        :background="false"
        :rotatable="false"
        :scalable="false"
        :zoomable="false"
        :zoom-on-touch="false"
        :zoom-on-wheel="false"
        :toggle-drag-mode-on-dblclick="false"
      />
      
      <button v-if="imageUrl" @click="getCroppedImage">Crop</button>
    </div>
    
    <div class="cropped-image-container" v-if="croppedImage">
      <h2>Cropped Image:</h2>
      <img :src="croppedImage" alt="Cropped Image" />
    </div>
  </div>
</template>

<script>
import VueCropper from 'vue-cropperjs';
import 'cropperjs/dist/cropper.css';

export default {
  components: {
    VueCropper
  },
  data() {
    return {
      imageUrl: '',
      croppedImage: null
    };
  },
  methods: {
    triggerFileUpload() {
      this.$refs.fileInput.click();
    },
    onFileChange(event) {
      const file = event.target.files[0];
      if (file) {
        const reader = new FileReader();
        reader.onload = (e) => {
          this.imageUrl = e.target.result;
        };
        reader.readAsDataURL(file);
      }
    },

    getCroppedImage() {
      const cropper = this.$refs.cropper;
      this.croppedImage = cropper.getCroppedCanvas().toDataURL();
    }
  }
};
</script>

<style scoped>
.container {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
}

.cropper-container {
  flex: 1;
  margin-right: 20px;
}

.cropped-image-container {
  flex: 1;
  text-align: center;
  width: 400px;
  height: 400px;
}

.cropped-image-container img {
  max-width: 100%;
  height: auto;
}
</style>
