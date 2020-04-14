<template>
  <div>
    <label class="check">
      <input v-model="what" type="checkbox">
      <span class="check__mark"></span>
      <div class="check__title">
        MEMENTO MORI
      </div>
    </label>
  </div>
</template>

<script>
  export default {
    computed: {
      what: {
        get: function () {
          // return
        },
        set: function (v) {
          this.$emit('checkbox', v)
        }
      }
    }
  }
</script>

<style scoped lang="scss">
  .check {
    width: 100%;
    height: 60px;

    display: flex;
    align-items: center;

    cursor: pointer;
    user-select: none;
    /*применяется для интерактивных элементов, на которые можно щёлкать,
    например, вкладки, и для которых выделение текста нежелательно.*/

    input {
      height: 0;
      width: 0;
    }

    &__mark {
      height: 20px;
      width: 18px;
      /*<!--background-color: $bg_blue;-->*/
      border: 1.5px solid $text__grey;
      box-sizing: border-box;
      border-radius: 4px;

      display: flex;
      align-items: center;
      justify-content: center;
    }

    &__mark:after {
      width: 18px; /*не более, чем .check__mark */
      height: 22px;
      background-image: url('data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAwAAAAJCAYAAAAGuM1UAAAACXBIWXMAAAsTAAALEwEAmpwYAAAAAXNSR0IArs4c6QAAAARnQU1BAACxjwv8YQUAAABvSURBVHgBlY+xCYAwEEXVYJPW1uzgDrY6hhNYO4KNq7iNjRO4g//DBQ6M5PLhkRDe53KusqcHrTPKM1iAb4zyBB5w1gXyzpMTRrCBLifzMX6JC62qlJQZLn3LfRCCTP3IscBcqhT+ZF3QJQ+OlMy8pG0XmQ2At8AAAAAASUVORK5CYII=');
      background-position: center;
      background-repeat: no-repeat;
      background-size: contain;
      content: '';
      display: none;
    }

    input:checked ~ &__mark:after {
      display: block;
    }

    &__title {
      padding-left: 10px;
    }
  }
</style>
