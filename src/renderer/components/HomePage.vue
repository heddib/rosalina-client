<template>
  <div id="container">
    <div id="inviteContainer">
      <div class="acceptContainer" v-if="!ready">
        <form @submit.prevent="setUsername">
          <h1>BIENVENUE !</h1>
          <div class="formContainer">
            <div class="formDiv" style="transition-delay: 0.2s;">
              <p>TON PSEUDO</p>
              <input type="text" required="" v-model:value="username" />
            </div>
            <div class="formDiv" style="transition-delay: 0.4s;">
              <button class="acceptBtn" type="submit">Commencer</button>
              <span class="register">Envie d'être anonyme ? <i class="twa twa-smirk"></i> <a @click="continueWithoutUsername">Rejoindre le chat</a></span>
            </div>
          </div>
        </form>
      </div>
      <div class="acceptContainer" v-if="ready">
        <form @submit.prevent="sendMessage">
          <h1>CHAT</h1>
          <div class="formContainer">
            <div class="formDiv">
              <ul id="chatbox">
                <li v-for="message in messages" :key="message.id">
                  <b>{{ message.user }}:</b> {{ message.message }}
                </li>
              </ul>
            </div>
            <div class="formDiv">
            <p>Message</p>
              <input type="text" required="" v-model:value="message" />
            </div>
            <div class="formDiv" style="opacity: 1; left: 0;">
              <button class="acceptBtn" type="submit" id="send">Envoyer</button>
            </div>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import io from 'socket.io-client'
import $ from 'jquery'
$.fn.transition = require('jquery.transit')

let socket = null

export default {
  name: 'home-page',
  methods: {
    sendMessage: function () {
      socket.emit('message', this.message)
      this.message = ''
    },
    setUsername: function () {
      socket.emit('join', this.username)
      this.username = ''
      this.ready = true
    },
    continueWithoutUsername: function () {
      let randomUser = 'Invité' + Math.floor(Math.random() * 2020) + 1
      socket.emit('join', randomUser)
      this.ready = true
    }
  },
  data () {
    return {
      messages: [],
      message: '',
      username: '',
      ready: false
    }
  },
  created () {
    // On se co
    socket = io('wss://rosalina-server.herokuapp.com/')
    // socket = io('http://127.0.0.1:3000')
  },
  mounted () {
    // Images aléatoires
    var images = [
      'https://canary.discordapp.com/assets/14c037b7102f18b2d2ccf065a52bb595.jpg'
    ]

    $('#send').attr('disabled')

    $('#container').append('<style>#container, .acceptContainer:before {background: url(' + images[Math.floor(Math.random() * images.length)] + ') center fixed }')
    // Animation
    setTimeout(function () {
      $('.acceptContainer').transition({
        height: '431.5px'
      })
      setTimeout(function () {
        $('.acceptContainer').addClass('loadIn')
        setTimeout(function () {
          $('.formDiv, form h1').addClass('loadIn')
        }, 500)
      }, 500)
    }, 500)

    // Petit hack
    var app = this

    socket.on('connect', function () {
      $('#send').attr('disabled', null)
    })

    // Update de la vue à chaque msg
    socket.on('message', function (message) {
      app.messages.push(message)
      // this needs to be done AFTER vue updates the page!!
      app.$nextTick(function () {
        var messageBox = document.getElementById('chatbox')
        messageBox.scrollTop = messageBox.scrollHeight
      })
    })
  }
}
</script>

<style>

@import url('../assets/twemoji.css');
@import url('/node_modules/normalize.css/normalize.css');

@import url('https://fonts.googleapis.com/css?family=Montserrat:400,600,700|Work+Sans:300,400,700,900');

* {
    outline-width: 0;
    font-family: 'Montserrat' !important;
    box-sizing: border-box;
	margin: 0;
	padding: 0;
}

body {
	background: #23272A;
}

::-webkit-scrollbar { /* scrollbar */
    width: 8px;
}
::-webkit-scrollbar-thumb { /* scrollbar */
    border-radius: 5px;
    background-color: #202225;
}
::-webkit-scrollbar-track {
    background: rgb(54, 57, 63);
}

li {
    list-style-type: none;
    color:rgba(255, 255, 255, 0.774)
}

#chatbox {
	height: 100px;
	max-width: 400px;
	overflow-y: scroll;
}

#container {
	height: 100vh;
	background-size: cover !important;
	display: flex;
	justify-content: center;
	align-items: center;
}

#inviteContainer {
	display: flex;
	overflow: hidden;
	position: relative;
	border-radius: 5px;
}

#inviteContainer .acceptContainer {
	padding: 45px 30px;
	box-sizing: border-box;
	width: 400px;
	margin-left: -400px;
	overflow: hidden;
	/* height: 0; */
	opacity: 0;
}

#inviteContainer .acceptContainer.loadIn {
	opacity: 1;
	margin-left: 0;
	transition: 0.5s ease;
}

#inviteContainer .acceptContainer:before {
	content: "";
	background-size: cover !important;
	box-shadow: inset 0 0 0 3000px rgba(40, 43, 48, 0.75);
	-webkit-filter: blur(10px);
	filter: blur(10px);
	position: absolute;
	width: 150%;
	height: 150%;
	top: -50px;
	left: -50px;
}

form {
	position: relative;
	text-align: center;
	height: 100%;
}

form h1 {
	margin: 0 0 15px 0;
	font-family: 'Work Sans' !important;
	font-weight: 700;
	font-size: 20px;
	color: #fff;
	-webkit-user-select: none;
	-moz-user-select: none;
	-ms-user-select: none;
	user-select: none;
	opacity: 0;
	left: -30px;
	position: relative;
	transition: 0.5s ease;
}

form h1.loadIn {
	left: 0;
	opacity: 1;
}

.formContainer {
	text-align: left;
}

.formContainer .formDiv {
	margin-bottom: 30px;
	left: -25px;
	opacity: 0;
	transition: 0.5s ease;
	position: relative;
}

.formContainer .formDiv.loadIn {
	opacity: 1;
	left: 0;
}

.formContainer .formDiv:last-child {
	padding-top: 10px;
	margin-bottom: 0;
}

.formContainer p {
	margin: 0;
	font-weight: 700;
	color: #aaa;
	font-size: 10px;
	-webkit-user-select: none;
	-moz-user-select: none;
	-ms-user-select: none;
	user-select: none;
}

.formContainer input[type=text],
.formContainer input[type=color],
.formContainer textarea {
	background: transparent;
	border: none;
	box-shadow: inset 0 -1px 0 rgba(255, 255, 255, 0.15);
	padding: 15px 0;
	box-sizing: border-box;
	color: #fff;
	width: 100%;
}

.formContainer input[type=text]:focus,
.formContainer input[type=color]:focus,
.formContainer textarea:focus {
	box-shadow: inset 0 -2px 0 #fff;
}

.logoContainer {
	padding: 45px 35px;
	box-sizing: border-box;
	position: relative;
	z-index: 2;
	position: relative;
	overflow: hidden;
	display: flex;
	align-items: center;
	justify-content: center;
	flex-direction: column;
	-webkit-transform: scale(0, 0);
	transform: scale(0, 0);
}

.logoContainer img {
	width: 150px;
	margin-bottom: -5px;
	display: block;
	position: relative;
}

.logoContainer img:first-child {
	width: 150px;
}

.logoContainer .text {
	padding: 25px 0 10px 0;
	margin-top: -70px;
	opacity: 0;
}

.logoContainer .text.loadIn {
	margin-top: 0;
	opacity: 1;
	transition: 0.8s ease;
}

.logoContainer .logo {
	position: relative;
	top: -20px;
	opacity: 0;
}

.logoContainer .logo.loadIn {
	top: 0;
	opacity: 1;
	transition: 0.8s ease;
}

.logoContainer:before {
	content: "";
	background-size: cover !important;
	position: absolute;
	top: -50px;
	left: -50px;
	width: 150%;
	height: 150%;
	-webkit-filter: blur(10px);
	filter: blur(10px);
	box-shadow: inset 0 0 0 3000px rgba(255, 255, 255, 0.8);
}

.forgotPas {
	color: #aaa;
	opacity: .8;
	text-decoration: none;
	font-weight: 700;
	font-size: 10px;
	margin-top: 15px;
	display: block;
	transition: 0.2s ease;
}

.forgotPas:hover {
	opacity: 1;
	color: #fff;
}

.acceptBtn {
	width: 100%;
	box-sizing: border-box;
	background: #7289DA;
	border: none;
	color: #fff;
	padding: 20px 0;
	border-radius: 3px;
	cursor: pointer;
	transition: 0.2s ease;
	-webkit-user-select: none;
	-moz-user-select: none;
	-ms-user-select: none;
	user-select: none;
}

.acceptBtn:hover {
	background: #6B7FC5;
}

.register {
	color: #aaa;
	font-size: 12px;
	padding-top: 15px;
	display: block;
}

.register a {
	color: #fff;
	text-decoration: none;
	margin-left: 5px;
	box-shadow: inset 0 -2px 0 transparent;
	padding-bottom: 5px;
	-webkit-user-select: none;
	-moz-user-select: none;
	-ms-user-select: none;
	user-select: none;
}

.register a:hover {
	box-shadow: inset 0 -2px 0 #fff;
}

</style>
