<template>
  <input name="search" id="search" :value="value" @input="handleChange" />
</template>
<script>
export default {
  name: 'SearchInput',
  props: {
    value: {
      type: String,
      required: true,
    },
  },
  methods: {
    handleChange(e) {
      this.$emit('input', e.target.value);
    },
  },
};
</script>

<style lang="scss" scoped>
input {
  margin-top: 50px;
  display: flex;
  flex-direction: column;
  width: 250px;
  color: white;
  text-align: center;
  font-size: 18px;
  font-weight: 400px;
  background-color: transparent;
  height: 30px;
  border: 0;
  border-bottom: 1px solid white;
  transition: box-shadow 0.5s;
}
input:focus {
  outline: none;
  box-shadow: 0 10px 20px -10px rgba(white, 0.5);
}
</style>
