<template>
  <li class="single-item">
    <nuxt-link
      class="single-item__link"
      :key="item.id"
      :to="{ name: 'items-id', params: { id: item.id } }"
    >
      <div class="single-item__info">
        <p class="single-item__property">{{ item.name }}</p>
        <p class="single-item__property">price: {{ item.price }}&#36;</p>
        <p class="single-item__property">date: {{ item.date }}</p>
      </div>
      <div class="single-item__controls">
        <app-action-btn
          :type="'edit'"
          @click.native.prevent="editItem"
        ></app-action-btn>
        <app-action-btn
          :type="'delete'"
          @click.native.prevent="deleteItem"
        ></app-action-btn>
      </div>
    </nuxt-link>
  </li>
</template>

<script>
import ActionBtn from '@/components/ui/ActionBtn';
export default {
  props: {
    item: {
      type: Object,
      required: true,
    },
  },
  components: {
    'app-action-btn': ActionBtn,
  },
  methods: {
    deleteItem() {
      this.$emit('deleteItem');
    },
    editItem() {
      this.$emit('editItem');
    },
  },
};
</script>

<style scoped>
/* .task */
.single-item__link {
  border: 1px solid #6f6f6f;
  border-radius: 5px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 43px 45px 44px 45px;
  text-decoration: none;
}

.single-item__controls {
  width: 126px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.single-item__property {
  margin: 0;
  font-weight: 900;
  font-size: 24px;
  line-height: 29px;
  color: #fff;
}
</style>
