<template>
  <div id="nav">
    <router-link id="logo_url" to="/">
        <img id="logo" :src="logo" :alt="alt">
    </router-link>
    <router-link to="/">Home</router-link>
    <router-link :to="{name: 'orders'}">Pedidos</router-link>
  </div>
</template>

<script>
export default {
    name: "Navbar-App",
    props: ["logo", "alt"]
}
</script>

<style scoped>
#nav {
    background-color:#ed303c;
    border-bottom: 4px solid #f7a541;
    padding: 15px 50px;
    display: flex;
    justify-content: flex-end;
    align-items: center;
}

#nav #logo_url {
    margin: auto;
    margin-left: 0;
}

#logo {
    width: 40px;
    height: 40px;
}

#nav a {
    color: #fff;
    text-decoration: none;
    font-size: 18px;
    margin: 12px;    
    transition: .5s;
}

#nav a:hover {
    color: #f7a541;
}
</style>