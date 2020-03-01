<template>
  <div>
    <paginate
      v-model="page"
      :click-handler="updatePage"
      :page-count="20"
      :page-range="5"
      :margin-pages="0"
      :prev-text="'Prev'"
      :next-text="'Next'"
      :container-class="'pagination'"
      :page-class="'page-item'"
      :prev-class="'prev-class'"
      :next-class="'next-class'"
    ></paginate>
  </div>
</template>

<script>
export default {
  props: {
    currentPage: {
      type: Number,
      required: true
    }
  },
  data: function() {
    return {
      page: this.currentPage + 1 //初期値の設定(watchしないと値は更新されない)
    };
  },
  watch: {
    currentPage: function(newValue) {
      // currentPageは親のページ数(リクエストのパラメータ）を反映しているので、
      // page(ページングの値)は +1 する必要がある。
      this.page = newValue + 1;
    }
  },
  methods: {
    updatePage() {
      // ページングは１始まり、APIのクエリのページ数は0始まりなので
      // クリックされた値を -1 して親（リクエストのパラメータ）に渡す必要がある
      this.$emit("update-page", this.page - 1);
    }
  }
};
</script>

<style>
/* styleにscopedをを入れるとページングの要素にスタイルが適用されなくなる */
a:focus {
  outline: none;
}

.page-item,
.prev-class,
.next-class {
  position: relative;
  display: block;
  padding: 0.5rem 0.75rem;
  margin-left: -1px;
  line-height: 1.25;
  color: #007bff;
  background-color: #fff;
  border: 1px solid #dee2e6;
}
.page-item.active {
  color: #fff;
  background-color: #007bff;
  border-color: #007bff;
}
.page-item.active a {
  color: inherit;
}

.pagination {
  width: 10%;
  margin: 0 auto;
}
</style>
