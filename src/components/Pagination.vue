<template>
  <div class="pagination">
    <button
      @click="changePage(1)"
      :disabled="currentPage === 1"
      class="pagination-button"
    >
      <i class="mdi mdi-skip-previous"></i>
    </button>
    <button
      @click="changePage(currentPage - 1)"
      :disabled="currentPage === 1"
      class="pagination-button"
    >
      <i class="mdi mdi-chevron-left"></i>
    </button>
    <button
      v-for="pageNumber in pages"
      :key="pageNumber"
      @click="changePage(pageNumber)"
      :class="['pagination-button', { active: pageNumber === currentPage }]"
    >
      {{ pageNumber }}
    </button>
    <button
      @click="changePage(currentPage + 1)"
      :disabled="currentPage === totalPages"
      class="pagination-button"
    >
      <i class="mdi mdi-chevron-right"></i>
    </button>
    <button
      @click="changePage(totalPages)"
      :disabled="currentPage === totalPages"
      class="pagination-button"
    >
      <i class="mdi mdi-skip-next"></i>
    </button>
  </div>
</template>

<script>
export default {
  props: {
    currentPage: Number,
    totalPages: Number,
  },
  computed: {
    pages() {
      const pageArray = [];
      for (
        let i = Math.max(1, this.currentPage - 2);
        i <= Math.min(this.currentPage + 2, this.totalPages);
        i++
      ) {
        pageArray.push(i);
      }
      return pageArray;
    },
  },
  methods: {
    changePage(page) {
      if (page >= 1 && page <= this.totalPages) {
        this.$emit("page-change", page);
      }
    },
  },
};
</script>

<style scoped>
.pagination {
  margin-top: 15px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.pagination-button {
  background-color: #007bff;
  color: white;
  border: 1px solid #ccc;
  padding: 5px 10px;
  cursor: pointer;
  margin: 0 2px;
  border-radius: 50%;
  transition: background-color 0.3s ease, color 0.3s ease;
}

.pagination-button.active {
  background-color: #f2f2f2;
  color: #007bff;
  border: 1px solid #007bff;
}

.pagination-button:hover:not([disabled]) {
  background-color: #ccc;
}

.mdi {
  font-size: 18px;
}

.mdi-skip-previous::before {
  content: "<<";
}

.mdi-chevron-left::before {
  content: "<";
}

.mdi-chevron-right::before {
  content: ">";
}

.mdi-skip-next::before {
  content: ">>";
}
</style>