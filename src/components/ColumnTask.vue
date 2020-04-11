<template>
	<div
		@click="goToTask(task)"
		@dragenter.prevent
		@dragover.prevent
		@dragstart="pickupTask($event,taskIndex,columnIndex)"
		@drop.stop="moveTaskOrColumn($event,column.tasks,columnIndex,taskIndex)"
		class="task"
		draggable="true"
	>
    <span class="w-full flex-no-shrink font-bold">
      {{ task.name }}
    </span>
		<p
			class="w-full flex-no-shrink mt-1 text-sm"
			v-if="task.description"
		>
			{{ task.description }}
		</p>
	</div>

</template>

<script>
  import movingTasksAndColumnsMixin from '@/mixins/movingTasksAndColumnsMixin.js'


  export default {
    name: "ColumnTask",
    mixins:[movingTasksAndColumnsMixin],
    props:{
      task: {
        type: Object,
        required: true
      },
      taskIndex: {
        type: Number,
        required: true
      },
	  },
	  methods:{
      goToTask(task) {
        this.$router.push({name: 'task', params: {id: task.id}})
      },
      pickupTask(e, taskIndex, fromColumnIndex) {
        e.dataTransfer.effectAllowed = 'move'
        e.dataTransfer.dropEffect = 'move'

        e.dataTransfer.setData('from-task-index', taskIndex)
        e.dataTransfer.setData('from-column-index', fromColumnIndex)
        e.dataTransfer.setData('type', 'task')
      },
	  }
  }
</script>

<style lang="css" scoped>
	.task {
		@apply flex items-center flex-wrap shadow mb-2 py-2 px-2 rounded bg-white text-grey-darkest no-underline;
		cursor: pointer;
	}
</style>