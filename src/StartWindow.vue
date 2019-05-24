<template>
  <div class="sc-start-window" :class="{opened: isOpen, closed: !isOpen}">
    <Header
      :title="title"
      :imageUrl="titleImageUrl"
      :onClose="onClose"
      :colors="colors"
      :disableUserListToggle="disableUserListToggle"
      @userList="handleUserListToggle"
    />
    <slot></slot>
  </div>
</template>

<script>
import Header from './Header.vue'

export default {
  components: {
    Header,
  },
  props: {
    title: {
      type: String,
      required: true
    },
    titleImageUrl: {
      type: String,
      default: ''
    },
    onClose: {
      type: Function,
      required: true
    },
    isOpen: {
      type: Boolean,
      default: () => false
    },
    colors: {
      type: Object,
      required: true
    },
    disableUserListToggle: {
      type: Boolean,
      default: false
    }
  },
  methods: {
    handleUserListToggle(showUserList) {},
  }
}
</script>
<style scoped>
.sc-start-window {
  width: 370px;
  height: calc(100% - 360px);
  max-height: 590px;
  position: fixed;
  right: 25px;
  bottom: 100px;
  box-sizing: border-box;
  box-shadow: 0px 7px 40px 2px rgba(148, 149, 150, 0.1);
  background: white;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  transition: 0.3s ease-in-out;
  border-radius: 10px;
  font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif;
}

.sc-start-window.closed {
  opacity: 0;
  visibility: hidden;
  bottom: 90px;
}

@media (max-width: 450px) {
  .sc-start-window {
    width: 100%;
    height: 100%;
    max-height: 100%;
    right: 0px;
    bottom: 0px;
    border-radius: 0px;
  }
  .sc-start-window {
    transition: 0.1s ease-in-out;
  }
  .sc-start-window.closed {
    bottom: 0px;
  }
}
</style>
