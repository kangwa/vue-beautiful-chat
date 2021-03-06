<template>
  <div :style="{background: backgroundColor}">
    <Header
      :chosenColor="chosenColor"
      :colors="colors"
    />
    <beautiful-chat
      :alwaysScrollToBottom="alwaysScrollToBottom"
      :close="closeChat"
      :colors="colors"
      :isOpen="isChatOpen"
      :messageList="messageList"
      :messageStyling="messageStyling"
      :newMessagesCount="newMessagesCount"
      :onMessageWasSent="onMessageWasSent"
      :open="openChat"
      :showStartWindow="showStartWindow"
      :participants="participants"
      :showEmoji="true"
      :showFile="true"
      :showTypingIndicator="showTypingIndicator"
      :titleImageUrl="titleImageUrl"
      @onType="handleOnType"
    >
    </beautiful-chat>
    <p class="text-center toggle">
      <a
        :style="{color: linkColor}"
        @click.prevent="openChat()"
        href="#"
        v-if="!isChatOpen"
      >Open the chat window</a>
      <a
        :style="{color: linkColor}"
        @click.prevent="closeChat()"
        href="#"
        v-else
      >Close the chat window</a>
    </p>
    <p class="text-center colors">
      <a
        :style="{background: availableColors.blue.launcher.bg}"
        @click.prevent="setColor('blue')"
        href="#"
      >Blue</a>
      <a
        :style="{background: availableColors.red.launcher.bg}"
        @click.prevent="setColor('red')"
        href="#"
      >Red</a>
      <a
        :style="{background: availableColors.green.launcher.bg}"
        @click.prevent="setColor('green')"
        href="#"
      >Green</a>
      <a
        :style="{background: availableColors.dark.launcher.bg}"
        @click.prevent="setColor('dark')"
        href="#"
      >Dark</a>
    </p>
    <v-dialog/>
    <p class="text-center messageStyling">
      <label>Message styling enabled?
        <input
          @change="messageStylingToggled"
          checked
          type="checkbox"
        >
      </label>
      <a
        @click.prevent="showStylingInfo()"
        href="#"
      >info</a>
    </p>
    <TestArea
      :chosenColor="chosenColor"
      :colors="colors"
      :messageStyling="messageStyling"
      :onMessage="sendMessage"
      :onTyping="handleTyping"
    />
    <Footer
      :chosenColor="chosenColor"
      :colors="colors"
    />
  </div>
</template>

<script>
import messageHistory from './messageHistory'
import chatParticipants from './chatProfiles'
import Header from './Header.vue'
import Footer from './Footer.vue'
import TestArea from './TestArea.vue'
import availableColors from './colors'

export default {
  name: 'app',
  components: {
    Header,
    Footer,
    TestArea
  },
  data() {
    return {
      participants: chatParticipants,
      titleImageUrl:
        'https://a.slack-edge.com/66f9/img/avatars-teams/ava_0001-34.png',
      messageList: messageHistory,
      newMessagesCount: 0,
      isChatOpen: false,
      showTypingIndicator: '',
      colors: null,
      availableColors,
      chosenColor: null,
      alwaysScrollToBottom: false,
      messageStyling: true,
      userIsTyping: false,
      showStartWindow: false,
    }
  },
  created() {
    this.setColor('blue')
  },
  methods: {
    sendMessage(text) {
      if (text.length > 0) {
        this.newMessagesCount = this.isChatOpen
          ? this.newMessagesCount
          : this.newMessagesCount + 1
        this.onMessageWasSent({
          author: 'support',
          type: 'text',
          data: { text }
        })
      }
    },
    handleTyping(text) {
      this.showTypingIndicator =
        text.length > 0
          ? this.participants[this.participants.length - 1].id
          : ''
    },
    onMessageWasSent(message) {
      this.messageList = [...this.messageList, message]
    },
    openChat() {
      this.isChatOpen = true
      this.newMessagesCount = 0
    },
    closeChat() {
      this.isChatOpen = false
    },
    setColor(color) {
      this.colors = this.availableColors[color]
      this.chosenColor = color
    },
    showStylingInfo() {
      this.$modal.show('dialog', {
        title: 'Info',
        text:
          'You can use *word* to <strong>boldify</strong>, /word/ to <em>emphasize</em>, _word_ to <u>underline</u>, `code` to <code>write = code;</code>, ~this~ to <del>delete</del> and ^sup^ or ¡sub¡ to write <sup>sup</sup> and <sub>sub</sub>'
      })
    },
    messageStylingToggled(e) {
      this.messageStyling = e.target.checked
    },
    handleOnType() {
      this.$root.$emit('onType')
      this.userIsTyping = true
    }
  },
  computed: {
    linkColor() {
      return this.chosenColor === 'dark'
        ? this.colors.sentMessage.text
        : this.colors.launcher.bg
    },
    backgroundColor() {
      return this.chosenColor === 'dark' ? this.colors.messageList.bg : '#fff'
    }
  }
}
</script>

<style>
body {
  padding: 0px;
  margin: 0px;
}

* {
  font-family: Avenir Next, Helvetica Neue, Helvetica, sans-serif;
}

.demo-description {
  max-width: 500px;
}

.demo-description img {
  max-width: 500px;
}

.demo-test-area {
  width: 300px;
  box-sizing: border-box;
}

.demo-test-area--text {
  box-sizing: border-box;
  width: 100%;
  margin: 0px;
  padding: 0px;
  resize: none;
  font-family: Avenir Next, Helvetica Neue, Helvetica, sans-serif;
  background: #fafbfc;
  color: #8da2b5;
  border: 1px solid #dde5ed;
  font-size: 16px;
  padding: 16px 15px 14px;
  margin: 0;
  border-radius: 6px;
  outline: none;
  height: 150px;
  margin-bottom: 10px;
}

.demo-monster-img {
  width: 400px;
  display: block;
  margin: 60px auto;
}

.text-center {
  text-align: center;
}

.colors a {
  color: #fff;
  text-decoration: none;
  padding: 4px 10px;
  border-radius: 10px;
}

.toggle a {
  text-decoration: none;
}

.messageStyling {
  font-size: small;
}

.demo-test-area--button {
  font-family: Avenir Next, Helvetica Neue, Helvetica, sans-serif;
  font-weight: 400;
  margin-top: 20px;
  user-select: none;
  border: none;
  line-height: 1.4;
  text-decoration: none;
  background: #4e8cff;
  color: white;
  padding: 6px 10px;
  font-size: 20px;
  height: 50px;
  border-radius: 4px;
  width: 80%;
  box-sizing: border-box;
  outline: none;
  cursor: pointer;
  align-self: center;
}

.demo-test-area--button:hover {
  background: #4883f1;
}

.login-form{
    height: 100%;
    overflow: auto;
    padding-left: 5px;
    padding-top: 8px;
}
</style>
