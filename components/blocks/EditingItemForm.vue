<template>
  <div class="editing-item-form">
    <p class="editing-item-form__title">{{ title }}</p>
    <form class="editing-item-form__form">
      <app-input
        type="text"
        v-model="name"
        required
        minlength="2"
        maxlength="30"
        class="editing-item-form__input"
        placeholder="Name..."
      />
      <app-input
        type="text"
        v-model="description"
        required
        class="editing-item-form__input"
        placeholder="Description..."
      />
      <app-input
        type="number"
        v-model="price"
        required
        class="editing-item-form__input"
        placeholder="Price..."
      />
      <app-input
        type="date"
        v-model="date"
        required
        class="editing-item-form__input"
        placeholder="Date..."
      />
      <app-button
        class="editing-item-form__button"
        theme="dark"
        @click.native.prevent="saveItem"
        >Save</app-button
      >
      <div
        class="editing-item-form__error"
        v-show="!isContentValid"
        ref="error"
      >
        <p class="editing-item-form__error-text">{{ errorText }}</p>
      </div>
    </form>
  </div>
</template>

<script>
import Input from '@/components/ui/Input';
import Button from '@/components/ui/Button';

export default {
  data() {
    return {
      name: '',
      price: '',
      date: '',
      description: '',
      isContentValid: true,
      errorText: 'Все поля должны быть заполнены',
    };
  },
  props: {
    action: {
      type: String,
      default: 'create',
    },
    id: {
      type: [String, Number],
      default: null,
    },
  },
  computed: {
    title() {
      switch (this.action) {
        case 'create':
          return 'Add item';
        case 'update':
          return 'Update item';
        default:
          return 'Add item';
      }
    },
  },
  components: {
    'app-input': Input,
    'app-button': Button,
  },
  methods: {
    saveItem() {
      if (this.name && this.price && this.date && this.description) {
        this.isContentValid = true;
        this.$store
          .dispatch('data-list/saveItem', {
            name: this.name,
            price: this.price,
            date: this.date,
            description: this.description,
            id: this.id,
          })
          .then(() => {
            this.$emit('submitted');
          });
      } else {
        this.isContentValid = false;
        this.errorText = 'Все поля должны быть заполнены.';
      }
    },
  },
  created() {
    if (this.id !== null) {
      const item = this.$store.getters['data-list/getItem'](this.id);
      if (item) {
        this.name = item.name;
        this.price = item.price;
        this.date = item.date;
        this.description = item.description;
        this.id = item.id;
        console.log(item);
      }
    }
  },
};
</script>

<style scoped>
.editing-item-form {
  min-width: 430px;
  min-height: 330px;
  background-color: #fff;
  border-radius: 10px;
  position: relative;
  box-sizing: border-box;
}

.editing-item-form__title {
  margin: 0;
  font-size: 24px;
  line-height: 30px;
  margin-bottom: 60px;
  text-transform: uppercase;
}

.editing-item-form__form {
  display: flex;
  flex-direction: column;
}

.editing-item-form__input {
  width: 100%;
  height: 47px;
  margin-bottom: 24px;
}

.editing-item-form__button {
  width: 100%;
  margin-top: 33px;
}

.editing-item-form__error {
  background-color: #f0f0f0;
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: 50px;
  margin-top: 30px;
}

.editing-item-form__error-text {
  color: #f00;
  font-size: 14px;
  line-height: 17px;
}
</style>
