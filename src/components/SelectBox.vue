<template>
  <div>
    <span>性別の指定：</span>
    <select @change="updateGender" v-model="gender">
      <option disabled value>性別</option>
      <option
        v-for="(optionValue, optionKey) in options"
        :key="optionValue"
        :value="optionKey"
      >{{optionValue}}</option>
    </select>
  </div>
</template>

<script>
export default {
  data() {
    return {
      gender: "",
      options: {
        "": "男女",
        male: "男性",
        female: "女性"
      }
    };
  },
  methods: {
    updateGender() {
      this.$emit("update-gender", this.gender);
    }
  }
};
</script>