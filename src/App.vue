<template>
  <div id="app">
    <navigation/>
    <div class="card">
      <div class="card_title">Esto no es un bot ~(ಠ︵ಠ)</div>
      <div class="card_content">
        <form v-on:submit.prevent="go">
          <div class="input_group">
            <label for="username">Twittear como:</label>
            <select name="username"
              id="username"
              v-model="username">
              <option :value="null">--- Selecciona un usuario ---</option>
              <option v-for="user in accounts"
                :key="user.username">
                {{ user.username }}
              </option>
            </select>
          </div>
          <div class="input_group">
            <label for="tweet">Contenido del tweet</label>
            <textarea name="tweet"
              id="tweet"
              cols="30"
              rows="10"
              v-model="tweet">
            </textarea>
          </div>
          <div class="input_group">
            <label for="quantity">Número de réplicas</label>
            <input type="number"
              name="quantity"
              id="quantity"
              v-model.number="quantity">
          </div>
          <div class="input_group">
            <button :disabled="!username">Envíar</button>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
  import axios from "axios"
  import Navigation from "./components/Navigation"
  export default {
    components: {
      Navigation
    },
    data() {
      return {
        accounts: [],
        tweet: "",
        username: null,
        quantity: 0
      }
    },
    computed: {
      selectedAccountConfig() {
        if (this.username) {
          const account = this.accounts.find(x => x.username == this.username)
          const defaults = {
            timeout_ms: 60 * 1000,
            strictSSL : true
          }
          return Object.assign(defaults, account.config)
        }
        return null
      }
    },
    methods: {
      go() {
        if (
          this.username && this.username.trim() != "" &&
          this.tweet && this.tweet.trim() != "" &&
          this.quantity && this.quantity.toString().trim() != ""
        ) {
          if (this.quantity > 0) {
            const payload = {
              config: this.selectedAccountConfig,
              tweet: {
                content: this.tweet,
                quantity: this.quantity
              }
            }
            axios.post("http://localhost:3000/api/post", payload)
              .then(({ data }) => {
                this.username = null
                this.tweet    = ""
                this.quantity = 0
                alert("Cadena propagada con éxito.")
              })
              .catch(error => alert("Ha ocurrido un error al ejecutar la operación."))
          } else {
            alert("El número de réplicas debe ser mayor a 0.")
          }
        } else {
          alert("Asegúrate de rellenar todos los campos.")
        }
      }
    }
  }
</script>

<style>
  @import url("https://fonts.googleapis.com/css?family=Open+Sans:400,700&display=swap");
  * {
    box-sizing : border-box;
    font-family: "Open Sans", sans-serif;
  } 
  body {
    margin : 0;
    padding: 0;
  }
  #app {
    align-items    : center;
    display        : flex;
    height         : 100vh;
    justify-content: center;
    background     : radial-gradient(circle, #266a64, #1b1b25);
    width          : 100vw;
  }
  .card {
    color    : #ffffff;
    max-width: 450px;
    padding  : 30px 24px;
    width    : 100%;
  }
  .card_title {
    font-size  : 24px;
    font-weight: bold;
  }
  .input_group {
    margin-top: 18px;
  }
  .input_group label {
    font-weight  : 700;
    margin-bottom: 6px;
    opacity      : .75;
  }
  .input_group * {
    display: block;
    outline: none;
    width  : 100%;
  }
  .input_group *:not(label) {
    -webkit-appearance: initial;
    background-color  : rgba(255, 255, 255, .1);
    border            : none;
    border-radius     : 3px;
    color             : #ffffff;
    font-size         : 14px;
    padding           : 6px 12px;
  }
  .input_group textarea {
    resize: none;
  }
  .input_group button {
    background-color: #266a64;
    box-shadow: 1px 1px 12px 3px rgba(0, 0, 0, 0.13);
    color: rgba(255, 255, 255, .75);
    cursor: pointer;
    font-size: 16px;
    font-weight: 700;
    letter-spacing: 1px;
    padding: 12px;
  }
  .input_group button:hover {
    color: #ffffff;
  }
  .input_group button:disabled {
    cursor: not-allowed;
  }
</style>