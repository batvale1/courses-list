<template>
  <div>
    <app-container>
      <app-data-list :itemsPerPage="2" />
    </app-container>
  </div>
</template>

<script>
import Container from '@/components/shared/Container';
import DataList from '~/components/blocks/DataList';
import EditingItemForm from '@/components/blocks/EditingItemForm';
export default {
  data() {
    return {
      metas: {
        meta_title: 'Courses list',
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
    'app-data-list': DataList,
    'app-editing-item-form': EditingItemForm,
  },
};
</script>

<style scoped></style>
