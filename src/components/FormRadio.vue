<template>
  <div>
      <label 
        v-for="value in values" 
        :key="value">
            {{value}}
            <input 
            type="radio" 
            :name="name"
            :value="value" 
            @change="$emit('input', $event.target.value)" /></label>
  </div>
</template>

<script>
export default {
    name: "FormRadio",
    props: {
        values: Array,
        name: String
    }
}
</script>

<style scoped lang="sass">

input
    width: fit-content
</style>