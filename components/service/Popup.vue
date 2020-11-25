<template>
  <div class="popup">
    <app-overlay @click="$emit('close')" />
    <div class="popup__content">
      <slot></slot>
      <img
        src="@/assets/images/close.svg"
        alt="Закрыть"
        class="popup__close"
        @click="$emit('close')"
      />
    </div>
  </div>
</template>

<script>
import Overlay from '@/components/service/Overlay';

export default {
  components: {
    'app-overlay': Overlay,
  },
};
</script>

<style scoped>
.popup {
}

.popup__content {
  position: fixed;
  top: 50%;
  left: 50%;
  z-index: 1000;
  transform: translate(-50%, -50%);
  max-height: 100vh;
  overflow: auto;
  display: inline-block;
  padding: 40px;
  vertical-align: bottom;
  background-color: #fff;
}

.popup__close {
  position: absolute;
  top: 40px;
  right: 40px;
  cursor: pointer;
}
</style>
