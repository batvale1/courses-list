<template>
  <div class="data-list">
    <h1 class="data-list__heading">Courses List</h1>
    <div class="data-list__actions">
      <app-button class="data-list__button" @click="addNewItem"
        >Add item</app-button
      >
      <app-sort-by
        :sortOptions="sortOptions"
        @selectOption="applySort"
      ></app-sort-by>
    </div>
    <ul class="data-list__list">
      <transition-group name="slide">
        <app-single-item
          class="data-list__item"
          v-for="item in list"
          :key="item.id"
          :item="item"
          @editItem="editItem(item.id)"
          @deleteItem="deleteItem(item.id)"
        />
      </transition-group>
    </ul>
    <app-pagination
      :recordsCount="listQuantity"
      :itemsPerPage="itemsPerPage"
      :pageChangeHandler="pageChangeHandler"
      :navBtnsQuantity="5"
      :navSideBtnsQuantity="2"
      v-model="currentPage"
    />
    <transition name="fade">
      <app-popup v-if="isEditingPopupShown" @close="closePopup">
        <app-editing-item-form
          :action="itemActionName"
          :id="currentItemId"
          @submitted="closePopup"
        />
      </app-popup>
    </transition>
  </div>
</template>

<script>
import Button from '@/components/ui/Button';
import SingleItem from '@/components/blocks/SingleItem';
import SortBy from '@/components/ui/SortBy';
import Pagination from '@/components/ui/Pagination';
import Popup from '@/components/service/Popup';
import EditingItemForm from '@/components/blocks/EditingItemForm';
export default {
  data() {
    return {
      selectedOption: 0,
      isEditingPopupShown: false,
      itemActionName: null,
      currentItemId: null,
      currentPage: 1,
    };
  },
  components: {
    'app-button': Button,
    'app-single-item': SingleItem,
    'app-sort-by': SortBy,
    'app-pagination': Pagination,
    'app-popup': Popup,
    'app-editing-item-form': EditingItemForm,
  },
  props: {
    itemsPerPage: {
      type: Number,
      default: 0,
    },
  },
  computed: {
    listQuantity() {
      return this.$store.getters['data-list/getListQuantity'];
    },
    list() {
      return this.$store.getters['data-list/getList'].slice(
        this.itemsPerPage * (this.currentPage - 1),
        (this.currentPage - 1) * this.itemsPerPage + this.itemsPerPage
      );
    },
    sortOptions() {
      return this.$store.getters['data-list/getSortOptions'];
    },
  },
  methods: {
    addNewItem() {
      this.itemActionName = 'create';
      this.currentItemId = '';
      this.isEditingPopupShown = true;
    },
    editItem(id) {
      this.itemActionName = 'update';
      this.currentItemId = id;
      this.isEditingPopupShown = true;
    },
    deleteItem(id) {
      this.$store.dispatch('data-list/deleteItem', { id });
    },
    applySort(e) {
      this.$store.dispatch('data-list/sortItems', { id: e.id });
    },
    closePopup() {
      this.isEditingPopupShown = false;
    },
    pageChangeHandler(pickedPage) {
      this.currentPage = pickedPage;
    },
  },
};
</script>

<style scoped>
.data-list {
  padding: 30px;
}

.data-list__heading {
  font-weight: 900;
  font-size: 60px;
  line-height: 73px;
  text-align: center;
  color: #fff;
  margin-bottom: 30px;
}

.data-list__list {
  list-style: none;
  margin: 0 0 30px 0;
  padding: 0;
  position: relative;
}

.data-list__item {
  margin-bottom: 10px;
}

.data-list__button {
  width: 150px;
  margin-bottom: 30px;
  text-transform: uppercase;
  font-weight: 700;
}

.data-list__actions {
  display: flex;
  justify-content: space-between;
}

.slide-enter {
  opacity: 0;
}

.slide-enter-active {
  animation: slide-in 1s ease-out forwards;
  transition: opacity 0.5s;
}

.slide-leave-active {
  animation: slide-out 1s ease-out forwards;
  transition: opacity 0.8s;
  opacity: 0;
  position: absolute;
  left: 0;
  right: 0;
}

.slide-move {
  transition: transform 0.8s;
}

@keyframes slide-in {
  from {
    transform: translateY(20px);
  }
  to {
    transform: translateY(0);
  }
}

@keyframes slide-out {
  from {
    transform: translateY(0);
  }
  to {
    transform: translateY(20px);
  }
}
</style>
