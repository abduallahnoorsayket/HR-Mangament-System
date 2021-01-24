<template>
  <!-- pagination -->
<ul class="pagination" >
    <li >
      <a href="#" >Showing {{this.paginate.showing}} of {{this.paginate.count}}</a>
    </li>
    <li :class="{disabled: !this.paginate.previous}">
      <a href="#!" v-on:click="$emit('get-data', paginate.previous)">Previous</a>
    </li>

    <li :class="{disabled: !this.paginate.next}">
      <a href="#!" v-on:click="$emit('get-data', paginate.next)">Next</a>
    </li>
  </ul>


</template>

<script>
export default {
  name: "Pagination",
  props: ["paginate"],
  created() {
      
      console.log(this.$props.paginate)
  }
};
</script>

<style scoped>
</style>