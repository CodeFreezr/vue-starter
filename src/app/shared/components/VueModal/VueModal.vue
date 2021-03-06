<template>
  <transition
    @beforeEnter="beforeEnter"
    @enter="enter"
    @beforeLeave="beforeLeave"
    @leave="leave">
    <div :class="$style.vueModal" v-if="show" ref="modal">
      <slot />
    </div>
  </transition>
</template>

<script lang="ts">
  import anime from 'animejs';

  export default {
    components: {},
    props: {
      show: {
        required: false,
        default: false,
        type: Boolean,
      },
    },
    data: function () {
      return {};
    },
    computed: {},
    methods: {
      beforeEnter(el: HTMLElement) {
        el.style.transform = 'translateY(100%)';

        document.getElementById('overlay').style.visibility = 'visible';
      },
      enter(el: HTMLElement, done: any) {
        document.getElementById('overlay').style.opacity = '0.5';
        anime({
          targets: el,
          translateY: {
            value: '0%',
            duration: 500,
            elasticity: 100,
          },
          complete: done
        });
      },
      beforeLeave(el: HTMLElement) {
        el.style.transform = 'translateY(0%)';
      },
      leave(el: HTMLElement, done: any) {
        const overlay: HTMLElement = document.getElementById('overlay');
        overlay.style.opacity = '0';

        anime({
          targets: el,
          translateY: {
            value: '200%',
            duration: 500,
            elasticity: 100,
          },
          complete() {
            overlay.style.visibility = 'hidden';
            done();
          }
        });
      },
      handleDocumentClick(e: Event) {
        if (this.$refs.modal && this.$refs.modal.contains(e.target) === false) {
          this.$emit('close');
        }
      },
    },
    beforeMount() {
      let overlay: HTMLElement = document.getElementById('overlay');

      if (overlay === null) {
        overlay = document.createElement('div');
        overlay.id = 'overlay';
        overlay.style.position = 'fixed';
        overlay.style.top = '0';
        overlay.style.left = '0';
        overlay.style.width = '100%';
        overlay.style.height = '100%';
        overlay.style.zIndex = '2000';
        overlay.style.background = '#000';
        overlay.style.opacity = '0';
        overlay.style.visibility = 'hidden';
        overlay.style.transition = 'opacity 500ms linear';
        document.body.appendChild(overlay);
      }

      document.addEventListener('click', this.handleDocumentClick);
      document.addEventListener('touchstart', this.handleDocumentClick);
    },
    beforeDestroy() {
      document.removeEventListener('click', this.handleDocumentClick);
      document.removeEventListener('touchstart', this.handleDocumentClick);
    }
  };
</script>

<style lang="scss" module>
  @import "../../variables";

  .vueModal {
    position:   fixed;
    top:        $grid-unit;
    left:       $grid-unit;
    bottom:     $grid-unit;
    right:      $grid-unit;
    background: $modal-bg;
    z-index:    $modal-index;
    box-shadow: $modal-shadow;
    padding:    $modal-padding;
  }

  @media(min-width: $screen-tablet-portrait) {
    .vueModal {
      width:       $modal-max-width;
      left:        50%;
      top:         0;
      bottom:      0;
      margin-left: -240px;
    }
  }
</style>
