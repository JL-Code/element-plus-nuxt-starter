<script setup>
const openDialog = () => {
  var dialogEl = document.getElementById("dialog");
  dialogEl.show();
};

const showModal = () => {
  var dialogEl = document.getElementById("dialog");
  dialogEl.showModal();
};

const initDialog = () => {
  var dialogEl = document.getElementById("dialog");
  dialogEl.addEventListener("close", () => {
    console.log("dialog close", dialogEl.returnValue);
  });
};

onMounted(() => {
  initDialog();
});
</script>

<template>
  <div>
    <button @click="openDialog">dialog show</button>
    <button @click="showModal">dialog showModal</button>

    <dialog id="dialog" class="my-dialog rounded-lg">
      <p>这是一个 dialog</p>
      <slot></slot>
      <form method="dialog" class="grid gap-x-8 gap-y-4 grid-cols-3">
        <button value="confirm" @click="($event) => $event.preventDefault()">
          Confirm
        </button>
        <button value="cancel" @click="($event) => $event.preventDefault()">
          Cancel
        </button>
      </form>
    </dialog>
  </div>
</template>

<style lang="scss" scoped>
.my-dialog {
  box-shadow: 2px 3px 4px 3px red;
}
dialog::backdrop {
  background: rgba(255, 0, 0, 0.25);
}
</style>
