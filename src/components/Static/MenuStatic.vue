<template>
  <div class="menu">
    <img class="menu--logo" src="../../../public/assets/logo.png" alt="logo">
    <hr class="menu--line" />
    <img class="menu--staticmenu" src="../../../public/assets/Menu.png" alt="menu">
  </div>
</template>

<script>

</script>

<style scoped lang="scss">
.menu {
  width: 300px;

  background-color: #0f1115;

  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 30px 0 15px 0;
  z-index: 100;
  
  .menu--logo {
    padding-bottom: 20px;
  }
  .menu--line {
    width: 233px;
    opacity: 20%;
    margin-bottom: 40px;
  }
}
</style>