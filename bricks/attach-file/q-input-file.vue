<template>
  <div class="error_wrap">
    <div class="image" @click="chooseImage">
      <div class="image_wrap" :class="{ image_error: error }">
        <img
          ref="filePreview"
          class="image_preview"
          :style="{'background-image': `url(${image ? image : imagePreview})` }"
        />
<!--        вместо <img> здесь можно использовать <div></div>-->
      </div>

      <input
        ref="fileInput"
        class="image_input"
        type="file"
        accept="image/*"
        @change="onSelectFile"
      />
      <div class="btn-title">Выбрать файл</div>
    </div>
  </div>
</template>

<script>
  // намного проще, чем в q-attach-file.
  // <input type="file"> - скрываем, рисуем произвольную зону для клика, и
  //  клик по зоне привязываем с инпутом через this.$refs.fileInput.click()

  export default {
    name: 'InputPhoto',
    props: {
      name: {
        type: String,
        default: null
      },
      error: {
        type: String,
        default: null
      },
      update: {
        type: Function,
        default: () => {
        }
      }
    },
    data: function () {
      return {
        imagePreview: require('~/static/imgs/image_bg.svg'),
        image: ''
      }
    },
    methods: {
      chooseImage() {
        this.$refs.fileInput.click()
      },
      onSelectFile(e) {
        const image = e.target.files[0]
        const reader = new FileReader()
        if (image) {
          reader.onload = e => {
            this.update(image)
            this.image = e.target.result
          }
        }
        reader.readAsDataURL(image)
      }
    },
    created() {
      this.$validator = this.$parent.$validator
    }
  }
</script>
<style lang="scss" scoped>
  .error {
    &_wrap {
      position: relative;
      display: grid;
    }
  }

  .image {
    position: relative;
    width: 100%;
    display: grid;
    grid-template-columns: auto 1fr;
    align-items: center;
    color: red;
    background-color: rgba(0, 0, 255, 0.21);

    &:hover {
      cursor: pointer;
    }

    &_wrap {
      width: 112px;
      height: 112px;
      border: 2px solid $text__grey;
      display: flex;
      align-items: center;
      justify-content: center;
      line-height: 1.25;
      @media screen and (max-width: 900px) {
        margin: auto;
      }
    }

    &_input {
      display: none;
      position: absolute;
      visibility: hidden;
    }

    &_preview {
      width: 40px;
      height: 40px;
      display: block;
      background-size: cover;
      background-position: center center;
    }

    &_error {
      border: 2px solid $error;
    }

    .btn-title {
      width: 150px;
      justify-self: start;
      text-align: left;
      border: orangered 1px dashed;
    }
  }
</style>
