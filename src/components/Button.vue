<template>
    <button @click="handleClick" :class="{ active: active }" :style="{ backgroundColor: color }"></button>
  </template>
  
  <script>
  export default {
    props: ['sound', 'color', 'disabled'],
    data() {
      return {
        active: false
      };
    },
    methods: {
      handleClick() {
        if (!this.disabled) {
          this.$emit('button-click', this.sound);
        }
      }
    }
  };
  </script>
  
  <style scoped>
  button {
    width: 200px;
    height: 200px;
    border-radius: 1px;
    cursor: pointer;
    border: none;
    transition: all 0.15s ease-out;

    &:nth-child(1) {
        border-top-left-radius: 50%;
    }
    &:nth-child(2) {
        border-top-right-radius: 50%;
    }
    &:nth-child(3) {
        border-bottom-left-radius: 50%;
    }
    &:nth-child(4) {
        border-bottom-right-radius: 50%;
    }

    &:hover {
        filter: brightness(1.2);
    }
  }
  </style>
  