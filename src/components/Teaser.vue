<template>
  <div class="teaser">
    <!--
      The _editable attribute makes the next
      DOM-element clickable so the sidebar can
      show the right component.
    -->
    <h1 v-editable="blok">
      <!--
       You can access every attribute you have defined in the schema 
       in the blok property, since this is the name we've passed it 
       in the HelloWorld.vue
      -->
      {{blok.headline}}
    </h1>
  </div>
</template>
  
<script>
export default {
  props: ['blok']
}
</script>