<template>
  <div>
    <Nuxt />
  </div>
</template>

<style lang="scss">
html {
  font-family:
    'Source Sans Pro',
    -apple-system,
    BlinkMacSystemFont,
    'Segoe UI',
    Roboto,
    'Helvetica Neue',
    Arial,
    sans-serif;
  font-size: 16px;
  word-spacing: 1px;
  -ms-text-size-adjust: 100%;
  -webkit-text-size-adjust: 100%;
  -moz-osx-font-smoothing: grayscale;
  -webkit-font-smoothing: antialiased;
  box-sizing: border-box;
}

*,
*::before,
*::after {
  box-sizing: border-box;
  margin: 0;
}

.arrow-left{
  background-image: url(../assets/arrow-right.png);
  background-repeat: no-repeat;
  background-size: contain;
  width: 50px;
  height: 50px;
  border: none;
  background-color: transparent;
  transform: rotate(180deg);
}
.arrow-right{
  background-image: url(../assets/arrow-right.png);
  background-repeat: no-repeat;
  background-size: contain;
  width: 50px;
  height: 50px;
  border: none;
  background-color: transparent;
}
.absolute{
  position: absolute;
}
.left{
  left: 0;
  top: 0;
  
}
.right{
  right: 0;
  top: 0;
}
.button{
  outline:none;
  &:hover{
    cursor: pointer;
  }
  &:active{
    background-color: gray;
  }
}
</style>
