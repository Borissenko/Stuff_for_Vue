<template>
  <div @click="onMenu"
       v-click-outside="onMenuDoun"
       class="nuv__hamburger"
  >
    <span class="span1" :class="{'span1_menu_up': menuIsUp}"></span>
    <span class="span2" :class="{'span2_menu_up': menuIsUp}"></span>
    <span class="span3" :class="{'span3_menu_up': menuIsUp}"></span>
  </div>
</template>

<script>
  import vClickOutside from 'v-click-outside'

  export default {
    name: "hamburger",
    data: () => ({
      menuIsUp: false
    }),
    methods: {
      onMenu() {
        this.menuIsUp ? this.menuIsUp = false : this.menuIsUp = true
      },
      onMenuDoun() {
        this.menuIsUp = false
      }
    },
    directives: {
      clickOutside: vClickOutside.directive
    },
  }
</script>

<style lang="scss" scoped>

  .nuv__hamburger {
    position: relative;
    left: 0;
    top: 4px;
    width: 26px;
    height: 30px;
    margin-left: auto;

    overflow: hidden;
    cursor: pointer;

    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;

    span {
      width: 100%;
      height: 2px;
      margin-bottom: 6px;
      background-color: black;
      border-radius: 4px;
      transition: all .5s ease-in;
    }

    .span1 {
      transform-origin: 4px 0px;
    }

    .span3 {
      transform-origin: bottom right;
    }

    .span1_menu_up {
      background-color: green;
      transform: rotate(44deg) translateX(4px) translateY(-1px);
    }

    .span2_menu_up {
      background-color: green;
      transform: rotate(135deg);
    }

    .span3_menu_up {
      background-color: green;
      transform: rotate(45deg);
      opacity: 0;
    }

    @media (min-width: 1020px) {
      display: none;
    }
  }

  * {
    transition: all .3s ease-in;
  }
</style>
