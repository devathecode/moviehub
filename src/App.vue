<template>
  <main>
    <router-view />
  </main>
</template>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Fira Sans', sans-serif;

  &::selection {
    background: transparentize(#42B883, 0.5);
  }
}

body {
  background-color: black;
}

a {
  text-decoration: none;
}

// Utilities

.flex {
  display: flex;
}

.flex-col {
  flex-direction: column;
}

.text-center {
  text-align: center;
}

.grid {
  display: grid;
}
</style>