<template>
  <input
    v-if="type == 'text' || type == 'date'" 
    :type="type" 
    :value="value" 
    :placeholder="placeholder"
    @input="$emit('input', $event.target.value)" />
  <input
    v-else-if="type == 'checkbox'" 
    :type="type" 
    :checked="value" 
    :placeholder="placeholder"
    @input="$emit('input', $event.target.checked)" />
</template>

<script>
export default {
    name: "FormInput",
    props: ['value', 'type', 'placeholder']
}
</script>

<style scoped lang="sass">
  input
    border: 0
    border-bottom: 1px solid rgba(0, 0, 0, 0.3)
    padding: 5px
    width: 100%

    &:focus
      outline: 0
      border-bottom: 2px solid rgba(0, 0, 0, 0.6)

  [type="checkbox"] 
    width: fit-content
    margin-left: 10px 
</style>