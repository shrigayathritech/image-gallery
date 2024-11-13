<template>
    <div class="image-card">
      <img :src="src" :alt="alt" @click="$emit('enlarge')" />
      <button @click="$emit('remove')">Remove</button>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      src: String,
      alt: String
    }
  }
  </script>
  
  <style>
  .image-card {
    position: relative;
    display: inline-block;
  }
  .image-card img {
    width: 150px;
    height: 150px;
    object-fit: cover;
  }
  button {
    position: absolute;
    top: 5px;
    right: 5px;
    background: #fff;
    border: none;
    cursor: pointer;
  }
  </style>
  