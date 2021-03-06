<template>
  <div :class="$style.stage" ref="stage">
    <canvas :class="$style.canvas" ref="canvas"></canvas>
    <div :class="$style.accent"></div>
    <div :class="$style.accentTwo"></div>

    <vue-grid>
      <vue-grid-row>
        <vue-grid-item class="vueGridItem">
          <h1 :class="$style.title">Tsipiripo</h1>
          <div :class="$style.subTitle">
            {{
            $t('App.core.description' /* A flexible, scalable, opinionated boilerplate for production-ready PWAs with
            focus on performance, development speed, and best practices */)
            }}
          </div>
        </vue-grid-item>
      </vue-grid-row>
    </vue-grid>
  </div>
</template>

<script lang="ts">
  import { CircleAnimation } from '../../shared/animations/CircleAnimation';
  import VueGrid             from '../../shared/components/VueGrid/VueGrid.vue';
  import VueGridRow          from '../../shared/components/VueGridRow/VueGridRow.vue';
  import VueGridItem         from '../../shared/components/VueGridItem/VueGridItem.vue';

  export default {
    components: { VueGridItem, VueGridRow, VueGrid },
    props:      {
      disableParticles: {
        type:     Boolean,
        required: true,
      },
    },
    data() {
      return {};
    },
    computed:   {},
    methods:    {
      handleResize() {
        const canvas: HTMLCanvasElement = this.$refs.canvas;
        const stage: HTMLElement = this.$refs.stage;
        const stageRect: ClientRect = stage.getClientRects().length > 0 ? stage.getClientRects().item(0) : {
          width:  0,
          height: 0,
        } as ClientRect;

        canvas.width = stageRect.width;
        canvas.height = stageRect.height;
      },
    },
    beforeMount() {
      window.addEventListener('resize', this.handleResize);
    },
    mounted() {
      this.handleResize();

      if (!this.disableParticles) {
        CircleAnimation(this.$refs.canvas);
      }
    },
    beforeDestroy() {
      window.removeEventListener('resize', this.handleResize);
    },
  };
</script>

<style lang="scss" module>
  @import "../../shared/styles";

  .stage {
    min-height: 100vh;
    background: linear-gradient(171deg, $brand-dark-primary 0%, $brand-primary 100%);
    overflow:   hidden;
    position:   relative;
    text-align: center;
    @include background-gradient($brand-dark-primary, $brand-primary, 171deg);
  }

  .accent {
    width:      56%;
    min-height: 100vh;
    transform:  skewX(-40deg) translateX(100%);
    box-shadow: $nav-bar-shadow;
    position:   absolute;
    transition: transform 250ms linear;
    @include background-gradient($brand-light-primary, $brand-dark-primary, -171deg);

    @include media(tabletPortrait) {
      transform: skewX(-33deg) translateX(100%);
    }

    @include media(tabletLandscape) {
      transform: skewX(-44deg) translateX(100%);
    }

    @include media(smallDesktop) {
      transform: skewX(-46deg) translateX(117%);
      width:     49%;
    }
  }

  .accentTwo {
    width:      56%;
    min-height: 100vh;
    transform:  skewX(30deg) translateX(-57%);
    box-shadow: $nav-bar-shadow;
    position:   absolute;
    transition: transform 250ms linear;
    @include background-gradient($brand-accent, $brand-light-primary, -171deg);

    @include media(tabletPortrait) {
      transform: skewX(37deg) translateX(-29%);
    }

    @include media(tabletLandscape) {
      transform: skewX(38deg) translateX(-29%);
    }

    @include media(smallDesktop) {
      transform: skewX(38deg) translateX(-19%);
      width:     49%;
    }
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
    top: $space-unit * 17;

    @include media(tabletPortrait) {
      top: $space-unit * 24;
    }

    @include media(tabletLandscape) {
      top: $space-unit * 26;
    }
  }

  .subTitle {
    top: $space-unit * 15;

    @include media(tabletPortrait) {
      top: $space-unit * 22;
    }

    @include media(tabletLandscape) {
      top: $space-unit * 24;
    }
  }

  .github {
    font-size:           $font-size-h1;
    top:                 $space-unit * 17;
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

    @include media(tabletPortrait) {
      top: $space-unit * 30;
    }

    @include media(tabletLandscape) {
      top: $space-unit * 42;
    }

    i {
      height: $font-size-h1;
      width:  $font-size-h1;
    }
  }
</style>
