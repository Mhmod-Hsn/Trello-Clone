<template>
	<div
		@dragenter.prevent
		@dragover.prevent
		@dragstart.self="pickupColumn($event,columnIndex)"
		@drop="moveTaskOrColumn($event,column.tasks,columnIndex)"
		class="column"
		draggable="true"
	>
		<div class="flex items-center mb-2 font-bold">
			{{ column.name }}
		</div>
		<div class="list-reset">

			<columnTask
				v-for="(task, $taskIndex) of column.tasks"
				:key="$taskIndex"
				:task="task"
				:taskIndex="$taskIndex"
				:columnIndex="columnIndex"
				:column="column"
				:board="board"
			/>

			<input
				@keyup.enter="createTask($event,column.tasks)"
				class="block p-2 w-full bg-transparent"
				placeholder="+ Enter New Task"
				type="text"
			>

		</div>
	</div>
</template>

<script>
	import columnTask from '@/components/ColumnTask'
	import movingTasksAndColumnsMixin from '@/mixins/movingTasksAndColumnsMixin.js'


	export default {
    name: "BoardColumn",
    components:{
      columnTask
    },
	  mixins:[movingTasksAndColumnsMixin],
    methods: {
      createTask(e, tasks) {
        this.$store.commit('CREATE_TASK', {
          tasks,
          name: e.target.value
        })
        e.target.value = ''
      },
      pickupColumn(e, fromColumnIndex) {
        e.dataTransfer.effectAllowed = 'move'
        e.dataTransfer.dropEffect = 'move'

        e.dataTransfer.setData('from-column-index', fromColumnIndex)
        e.dataTransfer.setData('type', 'column')
      },
      moveTaskOrColumn(e, toTasks, toColumnIndex, toTaskIndex) {
        const type = e.dataTransfer.getData('type')
        if (type === 'task') {
          this.moveTask(e, toTasks, toTaskIndex !== undefined ? toTaskIndex : toTasks.length)
        } else if (type === 'column') {
          this.moveColumn(e, toColumnIndex)
        }
      },
      pickupColumn(e, fromColumnIndex) {
        e.dataTransfer.effectAllowed = 'move'
        e.dataTransfer.dropEffect = 'move'

        e.dataTransfer.setData('from-column-index', fromColumnIndex)
        e.dataTransfer.setData('type', 'column')
      },
    }
  }
</script>

<style lang="css" scoped>
	.column {
		@apply bg-grey-light p-2 mr-4 text-left shadow rounded;
		min-width: 350px;
	}

</style>