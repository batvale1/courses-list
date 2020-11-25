<template>
  <button
    :class="['single-item__btn', `single-item__btn_type_${type}`]"
    type="button"
  />
</template>

<script>
export default {
  props: {
    type: {
      type: String,
      required: true,
    },
  },
};
</script>

<style scoped>
.single-item__btn {
  background: none;
  border: none;
  padding: 25px;
  margin: 0;
  cursor: pointer;
  stroke: white;
  width: 25px;
  height: 25px;
}

.single-item__btn:focus {
  outline: none;
}

.single-item__btn_type_edit {
  background-image: url('~assets/images/edit-icon.svg');
  background-repeat: no-repeat;
  background-position: center;
}

.single-item__btn_type_delete {
  background-image: url('~assets/images/delete-icon.svg');
  background-repeat: no-repeat;
  background-position: center;
}
</style>
