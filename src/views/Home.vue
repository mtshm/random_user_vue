<template>
  <div>
    <h1>ABCオンラインレッスン</h1>

    <select-box @update-gender="updateGender" />
    <!-- 通信失敗時の処理 -->

    <section v-if="hasError">アプリケーションで問題が発生しています</section>

    <!-- 通信成功時の表示 -->
    <section v-else>
      <!-- 通信中の表示 -->
      <div v-if="loading">通信中です</div>

      <!-- ０件の際の表示 -->
      <div v-else-if="empty">検索した条件のユーザーはいません</div>

      <!-- 通信完了時の処理 -->
      <div v-else>
        <div class="alignmentElements">
          <div v-for="(feature, people) in peoples" :key="people" class="widthElement">
            <b-card no-body v-bind:img-src="feature.picture.large" img-alt="Image" img-top>
              <h4 slot="header">{{feature.name.first}} {{feature.name.last}}</h4>
              <b-list-group flush>
                <b-list-group-item>{{feature.email}}</b-list-group-item>
                <b-list-group-item>{{feature.phone}}</b-list-group-item>
                <b-list-group-item>{{feature.location.city}}</b-list-group-item>
                <b-list-group-item>{{feature.location.state}}</b-list-group-item>
              </b-list-group>
            </b-card>
          </div>
        </div>
      </div>
    </section>
    <vuejs-paginate class="paginate" :current-page="queries.page" @update-page="updatePage" />
  </div>
</template>

<script>
import SelectBox from "@/components/SelectBox.vue";
import VuejsPaginate from "@/components/VuejsPaginate.vue";
import axios from "axios";

export default {
  components: {
    SelectBox,
    VuejsPaginate
  },
  data: function() {
    return {
      peoples: [],
      hasError: false,
      loading: true,
      //APIでから情報を取得する際のパラメータ
      queries: {
        page: 0,
        results: 20,
        gender: "",
        inc: "picture,name,gender,email,phone,location"
      }
    };
  },
  // 最初に実行する処理
  created: function() {
    this.searchTeacher();
  },
  computed: {
    empty() {
      return this.peoples.length === 0;
    }
  },
  watch: {
    queries: {
      handler() {
        this.searchTeacher();
      },
      deep: true
    }
  },
  methods: {
    searchTeacher() {
      this.loading = true;
      axios
        .get("https://randomuser.me/api/", { params: this.queries })
        .then(response => (this.peoples = response.data.results))
        .catch(() => (this.hasError = true))
        .finally(() => (this.loading = false));
    },
    updatePage(page) {
      this.$set(this.queries, "page", page);
    },
    updateGender(gender) {
      this.$set(this.queries, "page", 0);
      this.$set(this.queries, "gender", gender);
    }
  }
};
</script>

<style scoped>
[v-cloak] {
  display: none;
}
.alignmentElements {
  display: flex;
  flex-wrap: wrap;
}
.widthElement {
  width: 25%;
}
b-card {
  max-width: 20rem;
}
.paginate {
  margin-top: 25px;
}
</style>