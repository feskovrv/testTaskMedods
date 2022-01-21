<template>
  <select
    @change="$emit('input', $event.target.value)">
        <option disabled selected>Выберите:</option>
      <option 
        v-for="option in options" 
        :key="option" 
        :value="option">
            {{option}}
        </option>
  </select>
</template>

<script>
export default {
    name: "FormSelect",
    props: {
        options: Array
    },
}
</script>

<style scoped lang="sass">

select
    width: 100%
    border: none
    border-bottom: 1px solid rgba(0, 0, 0, 0.3)

    &:focus
      border: 0
      border-bottom: 2px solid rgba(0, 0, 0, 0.6)
      outline: 0

</style>