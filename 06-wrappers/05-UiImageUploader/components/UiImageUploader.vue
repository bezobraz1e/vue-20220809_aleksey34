<template>
  <div class="image-uploader">
    <label
      class="image-uploader__preview"
      :class="{ 'image-uploader__preview-loading': currentState === $options.uploaderStates.loading }"
      :style="currImage && `--bg-url: url('${currImage}')`"
    >
      <span class="image-uploader__text">{{ currentState.text }}</span>
      <input type="file" accept="image/*" class="image-uploader__input" ref="input" v-bind="$attrs" @change="changeEvent" @click="clickEvent"  />
    </label>
  </div>
</template>

<script>
export default {
  name: 'UiImageUploader',

  inheritAttrs: false,

  props: {
    preview: {
      type: String,
    },
    uploader: {
      type: Function
    },
  },

  data() {
    return {
      currentState: this.preview ? this.$options.uploaderStates.filled : this.$options.uploaderStates.empty,
      selectedImage: null
    };
  },

  emits: ['select', 'error', 'upload', 'remove'],

  computed: {
    currImage() {
      return this.preview || this.selectedImage;
    },
  },

  uploaderStates: {
    empty: {
      text: 'Загрузить изображение',
    },
    loading: {
      text: 'Загрузка...',
    },
    filled: {
      text: 'Удалить изображение',
    },
  },

  methods: {
    changeEvent(event) {
      const localFile = event.target.files[0];
      this.selectedImage = URL.createObjectURL(localFile);
      debugger;
    },
    clickEvent(event) {

    }
  },
};
</script>

<style scoped>
.image-uploader {
}

.image-uploader__input {
  opacity: 0;
  height: 0;
}

.image-uploader__preview {
  --bg-url: var(--default-cover);
  background-size: cover;
  background-position: center;
  background-image: linear-gradient(0deg, rgba(0, 0, 0, 0.4), rgba(0, 0, 0, 0.4)), var(--bg-url);
  border: 2px solid var(--blue-light);
  border-radius: 8px;
  transition: 0.2s border-color;
  cursor: pointer;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  max-width: 512px;
  height: 228px;
}

.image-uploader__text {
  color: var(--white);
  font-family: 'Nunito', sans-serif;
  font-weight: 600;
  font-size: 20px;
  line-height: 28px;
}

.image-uploader__preview:hover {
  border-color: var(--blue);
}

.image-uploader__preview.image-uploader__preview-loading {
  cursor: no-drop;
}
</style>
