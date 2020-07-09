<template>
<!--  Несколько чекбоксов С ДИЗАЙНЕРСКИМ интерфейсом, созданных по v-for,-->
<!--  дизайн-классы генерируются автоматически через функцию SCSS.-->

  <div class="answers">
    <div class="answers__tip">Можно выбрать несколько вариантов</div>

    <div v-for="(option, index) in optionsOfAnswers"
         :key="index"
         class="answers__item"
    >
      <label class="checkbox-label"
             :class="{chosenAnswer: chosenAnswer(option.id)}"
      >
        <input type="checkbox"
               :value="option.id"
               v-model="chosenAnswers"
               :id="`checkbox_${index}`"
        >
        <span :class="[`design_${index}`]"></span>

        <div class="answer-text">
          {{option.answer_text}}
        </div>
      </label>
      <div class="img" :class="{'img_bg': (index +1) === optionsOfAnswers.length}"></div>
    </div>

    <div @click="prepareAnswer" class="answers__btn">Дальше</div>
  </div>
</template>

<script>
  export default {
    name: "MultipleAnswersQuestion",
    data: () => ({
      chosenAnswers: [],
      optionsOfAnswers: [
        {
          id: '1',
          answer_text: 'yes',
          type: 'a'
        },
        {
          id: '2',
          answer_text: 'no',
          type: 'a'
        },
        {
          id: '3',
          answer_text: 'may_be',
          type: 'a'
        },
        {
          id: 'a',
          answer_text: '1',
          type: 'b'
        },
        {
          id: 'b',
          answer_text: '2',
          type: 'b'
        },
        {
          id: 'c',
          answer_text: '3',
          type: 'b'
        },
      ]
    }),
    methods: {
      chosenAnswer(option_id) {
        return this.chosenAnswers.includes(option_id)
      },
      prepareAnswer() {
        this.$emit('chosenAnswers', this.chosenAnswers)
      }
    }
  }
</script>

<style scoped lang="scss">
  .answers {
    display: flex;
    flex-flow: column nowrap;
    margin-top: rem(16);

    &__tip {
      display: flex;
      align-items: center;
      justify-content: flex-end;
      padding-right: rem(30);

      @extend %font_13;
    }

    &__item {
      display: flex;
      flex-flow: row nowrap;
      align-items: center;
      margin: rem(16) 0 0;
      padding: 0 rem(24) 0 rem(28);
      user-select: none;


      .checkbox-label {
        display: flex;
        align-items: center;
        width: calc(100% - 3.6rem);
        min-height: rem(56);
        margin-right: rem(12);
        background: linear-gradient(245.14deg, #8E74D7 7.83%, #5433B3 96.43%);
        border-radius: rem(16) rem(16) 0 rem(16);
        cursor: pointer;

        .answer-text {
          margin: rem(16) rem(18) rem(16) rem(8);
          @extend %font_16;
        }
      }

      .chosenAnswer {
        background: linear-gradient(90deg, #FCAA0D 0%, #FDC354 100%);
      }

      .img {
        padding: rem(20);
      }

      .img_bg {
        background: url("../../assets/imgs/narrator.png") center center;
        background-size: contain;
      }
    }

    &__btn {
      min-width: rem(288);
      height: rem(48);
      margin: rem(40) auto rem(54);
      background: transparent;
      border: 2px solid #FFFFFF;
      box-sizing: border-box;
      border-radius: rem(16);
      cursor: pointer;

      display: flex;
      align-items: center;
      justify-content: center;

      @extend %font_17;
    }
  }

  @for $i from 0 through 10 {
    #checkbox_#{$i} {
      width: 0;
      height: 0;
    }
    .design_#{$i} {
      display: flex;
      align-items: center;
      justify-content: center;
      min-width: 10px;
      height: 10px;
      margin-left: rem(16);
    }
    .design_#{$i}::after {
      width: 10px;
      height: 10px;
      background-image: url('data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAwAAAAJCAYAAAAGuM1UAAAACXBIWXMAAAsTAAALEwEAmpwYAAAAAXNSR0IArs4c6QAAAARnQU1BAACxjwv8YQUAAABvSURBVHgBlY+xCYAwEEXVYJPW1uzgDrY6hhNYO4KNq7iNjRO4g//DBQ6M5PLhkRDe53KusqcHrTPKM1iAb4zyBB5w1gXyzpMTRrCBLifzMX6JC62qlJQZLn3LfRCCTP3IscBcqhT+ZF3QJQ+OlMy8pG0XmQ2At8AAAAAASUVORK5CYII=');
      background-position: center;
      background-repeat: no-repeat;
      background-size: contain;
      content: '';
      display: none;
    }
    input#checkbox_#{$i}:checked ~ .design_#{$i}:after {
      display: block;
    }
  }

</style>