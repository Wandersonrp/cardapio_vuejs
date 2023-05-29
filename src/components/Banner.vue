<template>
  <div id="main-banner">
    <h1>Make Your Burguer</h1>
  </div>
</template>

<script>
export default {
name: "Banner-App"
}
</script>

<style scoped>
#main-banner {
    background-image: url('../../public/img/burger.jpg');
    background-position: 0 -250px;
    background-size: cover;
    height: 400px;
    display: flex;
    align-items: center;
    justify-content: flex-start;
}

#main-banner h1 {
    color: #f7a541;
    text-align: center;
    text-align: center;
    font-size: 47px;
    background-color: #222;
    padding: 20px 40px;
}
</style>