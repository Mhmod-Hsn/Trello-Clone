<template>
  <div class="board">
    <div class="flex flex-row items-start">
      <boardColumn
        v-for="(column, $columnIndex) of board.columns"
        :key="$columnIndex"
        :column="column"
        :columnIndex="$columnIndex"
        :board="board"
      />


      <div class="column flex">
        <input
	        @keyup.enter="createColumn($event)"
	        class="p-2 mr-2 flex-grow bg-transparent"
	        placeholder="+ New Column Name"
	        type="text">

      </div>
    </div>


    <div
      class="task-bg"
      v-if="isTaskOpen"
      @click.self="close"
    >
      <router-view/>
    </div>
  </div>
</template>

<script>
  import {mapState} from 'vuex'
  import BoardColumn from '@/components/BoardColumn'

export default {
  components:{
    BoardColumn
  },
  computed: {
    ...mapState(['board']),
    isTaskOpen () {
      return this.$route.name === 'task'
    }
  },
  methods: {
    createColumn(e){
      this.$store.commit('CREATE_COLUMN', {name: e.target.value});
      e.target.value=''
    },
    close () {
      this.$router.push({ name: 'board' })
    },
  }
}
</script>

<style lang="css">


.board {
  @apply p-4 bg-teal-dark h-full overflow-auto;
}

.task-bg {
  @apply pin absolute;
  background: rgba(0,0,0,0.5);
}

</style>
