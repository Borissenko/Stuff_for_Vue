<template>
  <div>
    <div class="groups_wrapper">
      <div class="explanation">Сопоставте варианты из левой колонки с вариантами, расположенными справо</div>
      <div v-if="attention" class="attention">сначало выбери вариант из левой группы</div>
      <div class="groupA">
        <div v-for="(itemA, index) in groupA"
             :key="index"
             @click="createExemplar(itemA.id)"
             class="item"
             :class="[itemAColor(index, itemA.id), {[itemAColor_firstClick(index)]: itemA.id === licenseForColor}]"
        >
          {{itemA.answer_text}}
        </div>
      </div>
      <div class="groupB">
        <div v-for="(itemB, ind) in groupB"
             :key="ind + 'ex'"
             @click="realisation(itemB.id)"
             class="item"
             :class="itemBColor(ind, itemB.id)"
        >
          {{itemB.answer_text}}
        </div>
      </div>
    </div>

    <pre>local_response = {{response}}</pre>
    <div @click="sendAnswerBack" class="my_btn">Выбор завершен</div>
  </div>
</template>

<script>
  export default {
    name: "MatchingQuestion",
    props: {
      answerOptions: {
        type: Array,
        required: true
      }
    },
    data: () => ({
      response: [],    // ['a2', 'b3', 'c1']
      // groupA: [
      //   {
      //     id: '1',
      //     answer_text: 'yes',
      //     type: 'a'
      //   },
      //   {
      //     id: '2',
      //     answer_text: 'no',
      //     type: 'a'
      //   },
      //   {
      //     id: '3',
      //     answer_text: 'may_be',
      //     type: 'a'
      //   }
      // ],
      // groupB: [
      //   {
      //     id: 'a',
      //     answer_text: '1',
      //     type: 'b'
      //   },
      //   {
      //     id: 'b',
      //     answer_text: '2',
      //     type: 'b'
      //   },
      //   {
      //     id: 'c',
      //     answer_text: '3',
      //     type: 'b'
      //   }
      // ],
      attention: false,
      exemplar: function () {
        this.attention = true
      },
      licenseForColor: null
    }),
    computed: {
      groupA() {
        return this.answerOptions.filter(it => it.type === 'a')
      },
      groupB() {
        return this.answerOptions.filter(it => it.type === 'b')
      }
    },
    methods: {
      creator(itemA_id) {           //первая фаза carry, заявление фенкции.
        return function (itemB_id) {
          this.response = this.response.filter(it => {    //удаляем из response все ответы, связанные с itemA и с itemB
            return !(it.includes(itemA_id) || it.includes(itemB_id))
          })
          this.response.push(itemA_id + itemB_id)
        }
      },
      createExemplar(itemA_id) {
        this.licenseForColor = itemA_id   //для подтверждения, что колор должен примениться именно к данному item'у.
        this.attention = false            //ВЫключаем предупреждение о необходимости начинать выбор с левой колонки
        this.exemplar = this.creator(itemA_id)   //вторая фаза carry, клик по группе А.
      },
      realisation(itemB_id) {      //третья фаза carry, клик по группе В.
        this.licenseForColor = null
        this.exemplar(itemB_id)      //запускаем третью фазу carry или, если экземпляр не сгенегирован, то вКЛЮЧАЕМ предупреждение о необходимости начинать выбор с левой колонки.
      },
      itemAColor_firstClick(index) {  //цвет itemA по первому клику
        return `color-bg${index}`
      },
      itemAColor(index, itemA_id) {
        //зависимость цвета itemA от index (выбор цвета) и наличия данного itemA_id в response (включенность цвета)
        let turnOn = false
        for (let item of this.response) {
          if (item.includes(itemA_id))
            turnOn = true
        }
        return {[`color${index}`]: turnOn}
      },
      itemBColor(ind, itemB_id) {
        //зависимость цвета itemB от itemA_id в response (выбор цвета и включенность цвета)
        let colorClassIndex
        let itemA_id
        if (this.response.length > 0) {
          let responseItem_id = this.response.find(it => it.includes(itemB_id))     //1. узнаем сопряженный itemA_id, из response
          if (responseItem_id)
            [itemA_id] = responseItem_id.match(/\d+/) || []

          colorClassIndex = this.groupA.findIndex(it => it.id === itemA_id)   //2. узнаем индекс для отбора цвета itemA
        }
        return [`color${colorClassIndex}`]
      },
      sendAnswerBack() {
        this.$emit('response', this.response)
      }
    }
  }
</script>

<style scoped lang="scss">
  .color0 {
    color: #c10ce2;
  }

  .color1 {
    color: red;
  }

  .color2 {
    color: blue;
  }

  .color3 {
    color: #77ff00;
  }

  .color4 {
    color: #eab909;
  }

  .color5 {
    color: #0fd7af;
  }

  .color6 {
    color: #faf248;
  }

  .color-bg0 {
    color: #c10ce2;
    background-color: rgba(255, 248, 220, 0.7);
  }

  .color-bg1 {
    color: red;
    background-color: rgba(255, 248, 220, 0.7);
  }

  .color-bg2 {
    color: blue;
    background-color: rgba(255, 248, 220, 0.7);
  }

  .color-bg3 {
    color: #77ff00;
    background-color: rgba(255, 248, 220, 0.7);
  }

  .color-bg4 {
    color: #eab909;
    background-color: rgba(255, 248, 220, 0.7);
  }

  .color-bg5 {
    color: #0fd7af;
    background-color: rgba(255, 248, 220, 0.7);
  }

  .color-bg6 {
    color: #faf248;
    background-color: rgba(255, 248, 220, 0.7);
  }

  .groups_wrapper {
    display: grid;
    grid-template-columns: 1fr 1fr;
    grid-template-rows: 70px max-content max-content;
  }

  .explanation {
    grid-column: 1 / span 2;
    grid-row: 1;
    align-self: center;
    padding: 0 7px;
  }

  .attention {
    grid-column: 1 / span 2;
    grid-row: 2;
    align-self: center;
    padding: 0 7px;
    color: orange;
  }

  .groupA {
    grid-column: 1;
    grid-row: 3;
  }

  .groupB {
    grid-column: 2;
    grid-row: 3;
  }

  .groupA, .groupB {
    display: flex;
    flex-flow: column nowrap;
    margin-top: 10px;
    padding: 7px;
    cursor: pointer;

    .item {
      margin-top: 7px;

      &:hover {
        text-decoration: underline;
      }
    }
  }

  pre {
    margin: 50px 7px 0;
    font-size: 14px;
  }

  .my_btn {
    cursor: pointer;
    border: #1aff00 1px solid;
    width: 200px;
    margin: 10px auto;
    padding: 7px;
  }

</style>