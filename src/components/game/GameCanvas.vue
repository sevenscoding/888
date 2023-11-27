<script setup>
import { ref, onMounted } from 'vue';

const isAnimationRunning = ref(false);
const card = ref(null);
const isFlipEnable = ref(true);

onMounted(() => {
  card.value.addEventListener('animationend', () => {
    isAnimationRunning.value = false;
    isFlipEnable.value = true;
  });
});

const flipACard = () => {
  isAnimationRunning.value = true;
  isFlipEnable.value = false;
}
</script>

<template>
  <div class="game-canvas">
    <div class="game-canvas__block">
        <div class="game-canvas__top">
          <div class="game-canvas__current-card"></div>
        </div>
        <div class="game-canvas__bottom">
          <div class="game-canvas__card"></div>
          <div class="game-canvas__card"></div>
          <div class="game-canvas__card"></div>
          <div ref="card" class="animated-card" :class="{ 'animated': isAnimationRunning }">
            <div class="animated-card__front"></div>
            <div class="animated-card__back"></div>
          </div>
        </div>
        <div class="game-canvas__btn" :class="{ 'disabled': !isFlipEnable }" @click="flipACard">
          flip a card
        </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.game-canvas {
  @include adaptive-bg;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  border: 1px solid var(--dark-blue-color);
  background-image: url('./src/assets/img/game-bg.png');
  background-color: var(--woodsmoke-color);

  @include phones {
    align-items: flex-start;
    padding-top: 40px;
    padding-bottom: 0;
  }

  &__btn {
    width: 18.229vw;
    height: 4.167vw;
    border-radius: 0.417vw;
    background-color: var(--dark-grey-color);
    cursor: pointer;
    display: flex;
    justify-content: center;
    align-items: center;
    margin: 0 auto;
    font-size: 1.667vw;
    font-weight: 800;
    text-transform: uppercase;
    transition: .3s background-color;
    &:hover {
      background-color: var(--toolbox-color);
    }
    &.disabled {
      pointer-events: none;
      cursor: not-allowed;
      opacity: 0;
    }

    @include phones {
      width: 200px;
      height: 40px;
      font-size: 16px;
    }
  }

  &__bottom {
    display: flex;
    justify-content: center;
    gap: 1.042vw;
    margin-bottom: 2.604vw;
    position: relative;
    perspective: 350px;

    @include phones {
      gap: 15px;
      margin-bottom: 20px;
      perspective: 500px;
    }
  }

  &__card {
    @include adaptive-bg;
    width: 9.635vw;
    height: 14.427vw;
    background-image: url('./src/assets/img/card-back-pattern.svg');
    position: relative;
    z-index: 1;

    @include phones {
      width: 70px;
      height: 105px;
    }
  }

  &__block {
    position: relative;
  }

  &__top {
    margin-bottom: 2.604vw;
    display: flex;
    justify-content: center;

    @include phones {
      margin-bottom: 40px;
    }
  }

  &__current-card {
    @include adaptive-bg;
    width: 9.635vw;
    height: 14.427vw;
    background-image: url('./src/assets/img/card-front-pattern.svg');

    @include phones {
      width: 118px;
      height: 178px;
    }
  }

  .animated-card {
    width: 9.635vw;
    height: 14.427vw;
    transition: transform 2s;
    transform-style: preserve-3d;
    transform-origin: left center;
    position: absolute;
    z-index: 10;
    will-change: transform;
    &.animated {
      animation: 4s 1 ease flippingCard;
    }

    @include phones {
      width: 70px;
      height: 105px;
    }

    &__back, &__front {
      position: absolute;
      width: 100%;
      height: 100%;
      backface-visibility: hidden;
    }

    &__back {
      @include adaptive-bg;
      background-image: url('./src/assets/img/card-back-pattern.svg');
    }

    &__front {
      @include adaptive-bg;
      width: 100%;
      height: 100%;
      background-image: url('./src/assets/img/card-front-pattern.svg');
      transform: rotateY( -180deg );
    }

  }
}
</style>

