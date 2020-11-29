<template>
  <div>
    <Nuxt />
  </div>
</template>

<style>
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

  background-color: #EFF0F0;
}

/*Adjusting selection highlight colours*/
::selection {
  background: rgba(113, 133, 236, 0.23); /* WebKit/Blink Browsers */
}
::-moz-selection {
  background: rgba(113, 133, 236, 0.23); /* Gecko Browsers */
}

*,
*::before,
*::after {
  box-sizing: border-box;
  margin: 0;
}


</style>
