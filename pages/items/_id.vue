<template>
  <div class="single-item">
    <app-container>
      <h1 class="single-item__heading">Purchase № {{ currentItem.id }}</h1>
      <p class="single-item__text">Name: {{ currentItem.name }}</p>
      <p class="single-item__text">
        Description: {{ currentItem.description }}
      </p>
      <p class="single-item__text">Price: {{ currentItem.price }} $</p>
      <p class="single-item__text">Date: {{ currentItem.date }}</p>
      <p class="single-item__text">Additional info and actions...</p>
      <app-button class="single-item__button" @click="editItem"
        >Edit item</app-button
      >
    </app-container>
    <transition name="fade">
      <app-popup v-if="isEditingPopupShown" @close="closePopup">
        <app-editing-item-form
          action="update"
          :id="currentItem.id"
          @submitted="closePopup"
        />
      </app-popup>
    </transition>
  </div>
</template>

<script>
import Container from '@/components/shared/Container';
import Popup from '@/components/service/Popup';
import EditingItemForm from '@/components/blocks/EditingItemForm';
import Button from '@/components/ui/Button';
export default {
  data() {
    return {
      isEditingPopupShown: false,
      metas: {
        meta_title: 'Single item',
        meta_description: 'тестовое задание',
        meta_keywords: 'тестовое задание',
      },
    };
  },
  head() {
    if (this.metas) {
      return {
        title: this.metas.meta_title,
        meta: [
          {
            hid: 'description',
            name: 'description',
            content: this.metas.meta_description || '',
          },
          {
            hid: 'keywords',
            name: 'keywords',
            content: this.metas.meta_keywords || '',
          },
          {
            hid: 'og:title',
            property: 'og:title',
            content: this.metas.meta_title || '',
          },
          {
            hid: 'og:description',
            property: 'og:description',
            content: this.metas.meta_description || '',
          },
        ],
      };
    }
  },
  components: {
    'app-container': Container,
    'app-popup': Popup,
    'app-editing-item-form': EditingItemForm,
    'app-button': Button,
  },
  computed: {
    currentItem() {
      return this.$store.getters['data-list/getCurrentSingleItem'];
    },
  },
  async fetch({ store, route, error }) {
    await store
      .dispatch('data-list/getSingleItem', { id: route.params.id })
      .catch(() => {
        error({ statusCode: 404, message: 'Item not found' });
      });
  },
  methods: {
    editItem() {
      this.isEditingPopupShown = true;
    },
    closePopup() {
      this.isEditingPopupShown = false;
    },
  },
};
</script>

<style scoped>
.single-item {
  color: #fff;
  margin: 30px;
}

.single-item__heading {
  margin-bottom: 30px;
}

.single-item__text {
  font-size: 20px;
  margin-bottom: 10px;
}

.single-item__text:last-of-type {
  margin-bottom: 0;
}

.single-item__button {
  width: 150px;
  margin-top: 30px;
  text-transform: uppercase;
  font-weight: 700;
}
</style>
