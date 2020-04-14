<template>
  <div class="wrapper">
    <div v-if="imageSrc == null" class="fl_upld">
      <label for="fl_inp">
        <input type="file"
               id="fl_inp"
               @change="pickUpFile"
               accept="image/*"
        >
        Прикрепить файл
      </label>
    </div>
    <div v-else class="fl_upld">
      Прикрепленный файл:
    </div>

    <div  v-if="imageSrc != null">
      <img :src="imageSrc" width="70" height="70">
    </div>
    <div class="file-name">{{name}}</div>
  </div>
</template>

<script>
  export default {
    name: "q-attach-file",
    data: () => ({
      name: '',
      imageSrc: null
    }),
    methods: {
      pickUpFile(e) {
        const pictures = e.target.files[0]
        this.name = pictures.name
        this.imageSrc = URL.createObjectURL(pictures)

        // const reader = new FileReader();
        // reader.onload = (e) => {
        //   this.imageSrc = e.target.result
        // }
        // reader.readAsDataURL(pictures);
      }
    }
  }
</script>

<style lang="scss" scoped>
  .wrapper {
    display: flex;
    flex-flow: row wrap;
    flex-flow: row wrap;
    justify-content: space-between;
  }

  #fl_inp {
    display: none;
  }

  .fl_upld {
    width: 35%;
    min-width: 210px;
    color: $secondary;
    display: flex;

    @media (max-width: 500px) {
      width: 100%;
    }

    label {
      padding: 0 0 0 60px;  /*<!--зона курсора и клика-->*/
      cursor: pointer;

      background: url("/imgs/skrepka.jpg") no-repeat left 50%;
      background-size: 40px;

      font-family: Suisse Intl;
      font-style: normal;
      font-weight: 400;
      font-size: 16px;
      line-height: 70px;
    }

    label:hover {
      color: #1A76DC;
    }
  }

  .file-name {
    width: 30%;
    display: flex;
    align-items: center;

    font-family: Suisse Intl;
    font-style: normal;
    font-weight: 400;
    font-size: 16px;
    line-height: 20px;
    color: $text__green;
    @media (max-width: 500px) {
      width: 100%;
    }

    /*.attachadImg {*/
    /*  width: 25%;*/
    /*  height: 70px;*/
    /*}*/
  }
</style>
