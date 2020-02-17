<template>
  <nav class="sidebar">
    <div class="menu_trigger">
      <a href="javascript:void(0)">
        <span></span>
        <span></span>
        <span></span>
      </a>
    </div>
    <ul class="menu">
      <li>
        <a v-on:click.prevent="getAllAccounts">Todas las cuentas</a>
      </li>
      <li>
        <a v-on:click.prevent="newAccount">Agregar nueva cuenta</a>
      </li>
      <li></li>
    </ul>
  </nav>
</template>

<script>
  export default {
    methods: {
      getAllAccounts() {
        console.log("Get all accounts")
      },
      newAccount() {
        console.log("Open new account modal")
      },
      navigation() {
        const triggerButton = document.querySelector(".menu_trigger a")
        const sidebar       = document.querySelector("nav.sidebar")
        const menuLinks = document.querySelectorAll("nav .menu a")
        menuLinks.forEach(link => {
          link.addEventListener("click", ev => {
            ev.preventDefault()
            sidebar.classList.remove("open")
          })
        })
        triggerButton.addEventListener("click", ev => {
          ev.preventDefault()
          sidebar.classList.contains("open")
            ? sidebar.classList.remove("open")
            : sidebar.classList.add("open")
        })
      }
    },
    mounted() {
      this.navigation()
    }
  }
</script>

<style scoped>
  nav.sidebar {
    align-items    : center;
    background     : #1b1b25;
    color          : #ffffff;
    display        : flex;
    height         : 100vh;
    justify-content: center;
    left           : -100vw;
    position       : absolute;
    transition     : left .5s;
    width          : 100vw;
    z-index        : 999;
  }
  nav.sidebar.open {
    left: 0px;
  }
  nav.sidebar.open .menu_trigger {
    right: 20px;
  }
  nav.sidebar .menu_trigger {
    align-items    : flex-end;
    display        : flex;
    flex-direction : column;
    height         : 60px;
    justify-content: center;
    position       : absolute;
    right          : -50px;
    top            : 0px;
    transition     : right .25s;
  }
  nav.sidebar .menu_trigger span {
    background-color: rgba(255, 255, 255, .85);
    display         : block;
    height          : 2px;
    margin-bottom   : 6px;
    width           : 25px;
  }
  ul.menu {
    list-style: none;
    padding   : 0px;
  }
  ul.menu li {
    margin-bottom: 24px;
    text-transform: uppercase;
  }
  ul.menu li a {
    color: rgba(255, 255, 255, .85);
    font-size: 24px;
    font-weight: 700;
    letter-spacing: .5px;
    text-decoration: none;
    transition: all .25s;
  }
  ul.menu li a:hover {
    text-shadow: 1px 1px 13px rgba(255, 255, 255, .85);
    text-decoration: underline;
  }
</style>