<template>
  <div class="buttons">
    <button class="blob-btn">
      {{ mensagem }}
      <span class="blob-btn__inner">
        <span class="blob-btn__blobs">
          <span class="blob-btn__blob"></span>
          <span class="blob-btn__blob"></span>
          <span class="blob-btn__blob"></span>
          <span class="blob-btn__blob"></span>
        </span>
      </span>
    </button>
    <br />
    <svg xmlns="http://www.w3.org/2000/svg" version="1.1">
      <defs>
        <filter id="goo">
          <feGaussianBlur
            in="SourceGraphic"
            result="blur"
            stdDeviation="10"
          ></feGaussianBlur>
          <feColorMatrix
            in="blur"
            mode="matrix"
            values="1 0 0 0 0 0 1 0 0 0 0 0 1 0 0 0 0 0 21 -7"
            result="goo"
          ></feColorMatrix>
          <feBlend in2="goo" in="SourceGraphic" result="mix"></feBlend>
        </filter>
      </defs>
    </svg>
  </div>
</template>

<script>
export default {
  props: {
    mensagem: String,
  },
};
</script>

<style lang="scss" scoped>
*,
*:before,
*:after {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

.buttons {
  margin-top: 50px;
  text-align: center;
  border-radius: 30px;
}

$cyan: #7179f4;
$dark: #ffffff;
$borderW: 2px;

.blob-btn {
  $numOfBlobs: 4;
  z-index: 1;
  position: relative;
  padding: 20px 60px;
  text-align: center;
  text-transform: uppercase;
  color: darken($color: $cyan, $amount: 10%);
  font-size: 16px;
  font-weight: bold;
  background-color: transparent;
  outline: none;
  border: none;
  transition: color 0.5s;
  cursor: pointer;
  border-radius: 30px;

  &:before {
    content: "";
    z-index: 1;
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    border: $borderW solid $cyan;
    border-radius: 30px;
  }

  &:after {
    content: "";
    z-index: -2;
    position: absolute;
    left: $borderW * 1.5;
    top: $borderW * 1.5;
    width: 100%;
    height: 100%;

    transition: all 0.3s 0.2s;
    border-radius: 30px;
  }

  &:hover {
    color: $dark;
    border-radius: 30px;
    cursor: pointer;

    &:after {
      transition: all 0.3s;
      left: 0;
      top: 0;
      border-radius: 30px;
    }
  }

  &__inner {
    z-index: -1;
    overflow: hidden;
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    border-radius: 30px;
    background: #ffffff;
  }

  // additional container created, because in FF blobs are breaking overflow:hidden of element with svg gooey filter
  &__blobs {
    position: relative;
    display: block;
    height: 100%;
    filter: url("#goo");
  }

  &__blob {
    position: absolute;
    top: $borderW;
    width: 100% / $numOfBlobs;
    height: 100%;
    background: $cyan;
    border-radius: 100%;
    transform: translate3d(0, 150%, 0) scale(1.7);
    transition: transform 0.45s;

    @supports (filter: url("#goo")) {
      transform: translate3d(0, 150%, 0) scale(1.4);
    }

    @for $i from 1 through $numOfBlobs {
      &:nth-child(#{$i}) {
        left: ($i - 1) * (120% / $numOfBlobs);
        transition-delay: ($i - 1) * 0.08s;
      }
    }

    .blob-btn:hover & {
      transform: translateZ(0) scale(1.7);

      @supports (filter: url("#goo")) {
        transform: translateZ(0) scale(1.4);
      }
    }
  }
}
</style>
