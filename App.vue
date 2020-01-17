<template>
  <div class="app">
    <form @submit.prevent="logIn">
      <h1>Login</h1>
      <input
        v-model="email"
        type="email"
        placeholder="Email Address"
        required
      >
      <input
        v-model="password"
        type="password"
        placeholder="Password"
        required
      >
      <button type="submit">
        LOG IN
      </button>
    </form>
  </div>
</template>

<script>
import { CometChat } from '@cometchat-pro/chat'

export default {
  data () {
    return {
      email: '',
      password: ''
    }
  },

  methods: {
    async logIn () {
      if (!CometChat.isInitialized()) {
        const appId = 'your-app-id'
        try {
          await CometChat.init(appId)
          console.log('CometChat was initialized successfully!')
        } catch (e) {
          console.log('An error occured while initializing CometChat')
        }
      }

      const response = await fetch(`http://localhost:3000/login?email=${this.email}&password=${this.password}`)
      if (!response.ok) {
        alert('Wrong email or password!')
      } else {
        const user = await response.json()
        await CometChat.login(user.cometchatAuthToken)
        alert('Logged in successfully!')
      }
    }
  }
}
</script>

<style>
* {
  box-sizing: border-box;
}

html, body, .app {
  padding: 0;
  margin: 0;
  height: 100%;
  width: 100%;
}

body {
  background: hsl(268, 76%, 97%);
  font-family: Roboto, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-rendering: optimizeLegibility;
}

.app {
  display: flex;
  justify-content: center;
  align-items: center;
}

form {
  margin: 0;
  padding: 40px 30px;
  border-radius: 3px;
  background: hsl(268, 76%, 99%);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  max-width: 350px;
  width: calc(100% - 20px);
  box-shadow: 0 2px 5px hsla(268, 56%, 30%, 20%);
}

input {
  border: none;
  padding: 10px 0;
  background: transparent;
  outline: none;
  margin-bottom: 20px;
  width: 100%;
  border-bottom: 2px solid hsl(268, 76%, 40%);
  font-size: 18px;
  color: hsl(268, 40%, 20%);
}

input::placeholder {
  color: hsl(268, 20%, 80%);
  font-weight: 300;
}

button {
  padding: 10px 30px;
  border: none;
  outline: none;
  background: hsl(268, 76%, 29%);
  color: hsl(268, 76%, 99%);
  border-radius: 38px;
  width: calc(100% - 40px);
  margin-top: 30px;
  font-size: 15px;
  cursor: pointer;
  transition: all 0.2s ease-in-out;
}

button:hover {
  background: hsl(268, 76%, 34%);
}

h1 {
  color: hsl(268, 76%, 29%);
  font-weight: 400;
  margin: 0 0 50px 0;
  font-size: 30px;
  text-transform: uppercase;
  align-self: flex-start;
  position: relative;
}

h1:before {
  content: '';
  position: absolute;
  height: 25px;
  width: 3px;
  background: hsl(268, 76%, 29%);
  left: -30px;
  top: 5px;
}
</style>
