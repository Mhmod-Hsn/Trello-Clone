<template>
  <div class="task-view" >
    <div class="flex flex-col flex-grow items-start justify-between px-4">

      <input
        type="text"
        class="p-2 mr-2 flex-grow text-xl font-bold w-full"
        :value="task.name"
        @change="updateTaskProperty($event,'name')"
        @keyup.enter="updateTaskProperty($event,'name')"
        @keyup.esc="closeTaskView"
      >
      <textarea
        class="relative bg-transparent px-2 border mt-2 h-64 border-none leading-normal w-full"
        :value="task.description"
        @change="updateTaskProperty($event,'description')"
        @keyup.enter.exact="updateTaskProperty($event,'description')"
        @keyup.esc="closeTaskView"

      ></textarea>
    </div>
  </div>
</template>

<script>
import { mapGetters } from 'vuex'

export default {
  computed: {
    ...mapGetters(['getTask']),
    task () {
      return this.getTask(this.$route.params.id)
    },
  },
  methods:{
    updateTaskProperty(e,key){
      this.$store.commit('UPDATE_TASK',{
        task:   this.task,
        key:    key,
        value:  e.target.value
      })
      if (e.key === 'Enter'){
        this.$router.push({ name: 'board' })
      }
    },
    closeTaskView(){
      this.$router.push({ name: 'board' })
    }
  }
}
</script>

<style>
.task-view {
  @apply relative flex flex-row bg-white pin mx-4 m-32 mx-auto py-4 text-left rounded shadow;
  max-width: 700px;
}
</style>
