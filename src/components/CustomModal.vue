<template>
  <div class="modal-overlay" @click="close">
    <div class="modal-content" @click.stop>
      <p>{{ message }}</p>
      <button @click="close" class="modal-button">閉じる</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "CustomModal",
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
  background: rgba(0, 0, 0, 0.5); /* 背景を透過 */
  display: flex;
  align-items: center;
  justify-content: center;
}
.modal-content {
  background: #cecece; /* モーダルの内容部分は不透過 */
  padding: 20px;
  border-radius: 8px;
  text-align: center;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}
.modal-content p {
  margin-bottom: 20px;
  color: #000; /* 文字は不透過で濃い色 */
}
.modal-button {
  background-color: #cecece; /* ボタンの背景色 */
  border: 1px solid #aaa; /* ボタンの枠線 */
  padding: 10px 20px; /* ボタンの内側余白 */
  border-radius: 4px; /* ボタンの角を丸くする */
  cursor: pointer; /* マウスカーソルをポインターに変更 */
  transition: background-color 0.3s ease; /* 背景色変更のアニメーション */
}
.modal-button:hover {
  background-color: #bfbfbf; /* ホバー時の背景色 */
}
.modal-button:active {
  background-color: #a9a9a9; /* クリック時の背景色 */
}
</style>