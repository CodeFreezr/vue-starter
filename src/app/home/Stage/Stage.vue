<template>
  <div :class="$style.stage" ref="stage">
    <canvas :class="$style.canvas" ref="canvas"></canvas>
    <div :class="$style.accent"></div>
    <div :class="$style.accentTwo"></div>

    <h1 :class="$style.title">Vue.js Starter</h1>
    <div :class="$style.subTitle">an enterprise ready boilerplate for isomorphic, progressive web apps</div>

    <a :class="$style.github" href="https://github.com/devCrossNet/vue-starter" target="_blank" rel="noopener">
      <i class="fab fa-github"></i>
    </a>
  </div>
</template>

<script lang="ts">
  import { CircleAnimation } from '../../shared/animations/CircleAnimation';

  export default {
    components: {},
    props: {},
    data() {
      return {};
    },
    computed: {},
    methods: {
      handleResize() {
        const canvas: HTMLCanvasElement = this.$refs.canvas;
        const stage: HTMLElement = this.$refs.stage;
        const stageRect: ClientRect = stage.getClientRects().length > 0 ? stage.getClientRects().item(0) : { width: 0, height: 0 } as ClientRect;

        canvas.width = stageRect.width;
        canvas.height = stageRect.height;
      }
    },
    beforeMount() {
      window.addEventListener('resize', this.handleResize);
    },
    mounted() {
      this.handleResize();

      CircleAnimation(this.$refs.canvas);
    },
    beforeDestroy() {
      window.removeEventListener('resize', this.handleResize);
    }
  };
</script>

<style lang="scss" module>
  @import "../../shared/variables";

  .stage {
    min-height: 100vh;
    background: linear-gradient(171deg, $brand-dark-primary 0%, $brand-primary 100%);
    overflow:   hidden;
    position:   relative;
    text-align: center;

    .accent {
      background: linear-gradient(-171deg, $brand-light-primary 0%, $brand-dark-primary 100%);
      width:      56%;
      min-height: 100vh;
      transform:  skewX(-40deg) translateX(100%);
      box-shadow: $nav-bar-shadow;
      position:   absolute;
      transition: transform 250ms linear;
    }

    .accentTwo {
      background: linear-gradient(-171deg, $brand-accent 0%, $brand-light-primary 100%);
      width:      56%;
      min-height: 100vh;
      transform:  skewX(30deg) translateX(-57%);
      box-shadow: $nav-bar-shadow;
      position:   absolute;
      transition: transform 250ms linear;
    }

    .canvas {
      min-height:       100vh;
      width:            100%;
      position:         absolute;
      background-color: transparent;
      left:             0;
      top:              0;
    }

    .title, .subTitle, .github {
      text-shadow: 0 5px 10px rgba(0, 0, 0, 0.33);
      position:    relative;
    }

    .title {
      top: $grid-unit * 17;
    }

    .subTitle {
      top: $grid-unit * 15;
    }

    .github {
      font-size:           $font-size-h1;
      top:                 $grid-unit * 17;
      box-shadow:          $nav-bar-shadow;
      display:             inline-block;
      background:          $nav-bar-bg;
      transition:          $button-transition;
      transition-property: box-shadow, background-color;
      color:               $text-color;
      padding:             $button-padding;

      &:hover {
        box-shadow: $button-active-shadow;
      }
    }
  }

  @media (min-width: $screen-tablet-portrait) {
    .stage {
      .accent {
        transform: skewX(-33deg) translateX(100%);
      }

      .accentTwo {
        transform: skewX(37deg) translateX(-29%);
      }

      .title {
        top:       $grid-unit * 24;
        font-size: $font-size-h1 + 4;
      }

      .subTitle {
        top: $grid-unit * 22;
      }

      .github {
        top: $grid-unit * 30;
      }
    }
  }

  @media (min-width: $screen-tablet-landscape) {
    .stage {
      .accent {
        transform: skewX(-44deg) translateX(100%);
      }

      .accentTwo {
        transform: skewX(38deg) translateX(-29%);
      }

      .title {
        top:       $grid-unit * 26;
        font-size: $font-size-h1 + 6;
      }

      .subTitle {
        top: $grid-unit * 24;
      }

      .github {
        top: $grid-unit * 42;
      }
    }
  }

  @media (min-width: $screen-small-desktop) {
    .stage {
      .accent {
        transform: skewX(-46deg) translateX(117%);
        width:     49%;
      }

      .accentTwo {
        transform: skewX(38deg) translateX(-19%);
        width:     49%;
      }
    }
  }
</style>
