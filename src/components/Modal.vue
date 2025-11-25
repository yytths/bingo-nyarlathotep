<template>
    <div class="modal-overlay" @click="close">
      <div class="modal-content" @click.stop>
        <p>{{ message }}</p>
        <button @click="close">閉じる</button>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: "Modal",
    props: {
      message: {
        type: String,
        required: true,
      },
    },
    methods: {
      close() {
        this.$emit("close");
      },
    },
  };
  </script>
  
  <style scoped>
  .modal-overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.5);
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .modal-content {
    background: #fff;
    padding: 20px;
    border-radius: 8px;
    text-align: center;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  }
  .modal-content p {
    margin-bottom: 20px;
  }
  </style>