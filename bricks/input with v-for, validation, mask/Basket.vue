<template>
  <div class="wrapper">
    <div v-for="(field, key, ind) in forms" :key="ind"> <!-- итерация по ОБЪЕКТУ(!)-->
      <input type="text"
             v-model="field.value"
             :placeholder="field.placeholder"
             v-mask="field.mask ? field.mask : ''"
             @blur="$v.forms[key].value.$touch()"
             :class="{'input-error': $v.forms[key].value.$error || isUntouchedFields,
                      'input-valid': !$v.forms[key].value.$invalid
                     }"
      >
    </div>

    <hr>
    <div v-for="(item, ind) in myArray" :key="'arr' + ind"> <!-- итерация по МАССИВУ-->
      <input type="text"
             v-model="item['value']"
             :placeholder="item['placeholder']"
             v-mask="item['mask'] ? item['mask'] : ''"
      >
    </div>
    <div @click="onRapport" class="control">
      Отправить
    </div>
  </div>
</template>

<script>
import AwesomeMask from 'awesome-mask'   //npm i awesome-mask , + регистрация в directives: {}.
import {required} from 'vuelidate/lib/validators'
import isPhone from '@/assets/utils/validation'

export default {
  data: () => ({
    isUntouchedFields: false,
    forms: {       //итерация по ОБЪЕКТУ.
      name: {
        value: '',
        placeholder: 'Ваше имя'
      },
      phone: {
        value: '',
        placeholder: 'Телефон',
        mask: '(999) 999-99-99'
      }
    },
    myArray: [     //итерация по МАССИВУ. Но валидация по массиву - не получиться.
      {
        value: '',
        placeholder: 'Ваше имя'
      },
      {
        value: '',
        placeholder: 'Телефон',
        mask: '(999) 999-99-99'
      }
    ]
  }),
  validations: {
    forms: {
      name: {
        value: {required}
      },
      phone: {
        value: {isPhone}
      }
    }
  },
  methods: {
    onRapport() {
      let dirtyAll = Boolean(this.forms.name.value && this.forms.phone.value && this.forms.address.value)
      let noErrors = !this.$v.forms.$anyError

      //если отправляем, а к полям даже не дотрагивались - все поля зажгутся красным.
      if(!dirtyAll)
        this.isUntouchedFields = true

      //Отправка заказа на сервер.
      if(dirtyAll && noErrors) {
        this.POST_DATA_TO_SERVER(this.forms)
        this.$v.$reset()              //обнуление результатов валидации
        this.cleanFormsFields()       //очистка полей формы
      }
    }
  },
  directives: {
    'mask': AwesomeMask
  }
}
</script>

<style scoped lang="scss">

$black-extra: #000000;
$black: #1F1F1F;
$grey: #59606D;
$grey-light: #959DAD;
$grey-extra-light: #F8F8F8;
$white: #ffffff;
$error: #ff0000;
$valid: #008000;

.wrapper {
  box-sizing: border-box; // это важные свойства
  width: 100%;
  height: fit-content;
  min-height: rem(600);
  padding: rem(52) rem(48); //
  background: $white;
  @extend .panel-bs;
  border-radius: rem(8) 0 0 rem(8);

  input {
    width: calc(100% - 1.12rem);
    height: rem(50);
    margin-top: rem(16);

    background: $grey-extra-light;
    border-radius: rem(8);
    border: transparent 1px solid; //
    padding-left: rem(14);
    @extend .font_16;
    color: $black;

    & ::placeholder { //
      color: $grey-extra-light;
    }

    &:focus { //
      outline: 0; //
    }
  }

  .input-error {
    border: $error 1px solid; //
    color: $error;
  }

  .input-valid {
    border: $valid 1px solid;
    color: $valid;
  }
}

.control {
  width: 100%;
  height: rem(50);
  margin-top: rem(24);
  border-radius: rem(8);
  background: $black;
  cursor: pointer;

  @extend .flex-center;
  @extend .font_16;
  color: $white;

  &:hover {
    background: $grey;
  }
}
</style>
