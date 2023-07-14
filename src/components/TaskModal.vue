<template>
  <div class="modal-overlay">
    <div class="modal">
    <h3> Create new task </h3>
      <div>
        <slot></slot>
      </div>
    </div>

  </div>
</template>

<script>
export default {
}
</script>

<style scoped>

.modal-overlay {
  position: fixed;
  z-index: 1;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  display: flex;
  overflow: auto; /* Enable scroll if needed */
  justify-content: center;
  background-color: rgba(0,0,0,0.4);
}

.modal {
  text-align: left;
  background-color: white;
  height: 420px;
  width: 300px;
  margin-top: 10%;
  padding: 30px 0 0 30px;
  border-radius: 20px;
}
.close {
  margin: 10% 0 0 16px;
  cursor: pointer;
}

.close-img {
  width: 25px;
}

h6 {
  font-weight: 500;
  font-size: 28px;
  margin: 20px 0;
}

p {
  font-size: 16px;
  margin: 20px 0;
}

</style>