<template>
	<div
		@drop.self="onDrop"
		@dragenter.prevent
		@dragover.prevent

	>
		<slot/>
	</div>
</template>

<script>
  export default {
    name: "AppDrop",
	  methods:{
      onDrop(e){
        const transferDate = JSON.parse(e.dataTransfer.getData('payload'))

	      this.$emit('drop',transferDate)
      }
	  }
  }
</script>

<style scoped>

</style>