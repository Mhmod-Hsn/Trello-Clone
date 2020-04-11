<template>
	<AppDrop
		@drop="moveTaskOrColumn"
	>
		<AppDrag
			:transferData="{
            type: 'column',
            fromColumnIndex: columnIndex
          }"
			class="column"
		>
			<div class="flex items-center mb-2 font-bold">
				{{ column.name }}
			</div>
			<div class="list-reset">
				<ColumnTask
					:board="board"
					:column="column"
					:columnIndex="columnIndex"
					:key="$taskIndex"
					:task="task"
					:taskIndex="$taskIndex"
					v-for="(task, $taskIndex) of column.tasks"
				/>

				<input
					@keyup.enter="createTask($event, column.tasks)"
					class="block p-2 w-full bg-transparent"
					placeholder="+ Enter new task"
					type="text"
				/>
			</div>
		</AppDrag>
	</AppDrop>
</template>

<script>
  import ColumnTask from './ColumnTask'
  import AppDrag from './AppDrag'
  import AppDrop from './AppDrop'
  import movingTasksAndColumns from '@/mixins/movingTasksAndColumnsMixin'

  export default {
    components: {
      ColumnTask,
      AppDrag,
      AppDrop
    },
    mixins: [movingTasksAndColumns],
    methods: {
      pickupColumn(e, fromColumnIndex) {
        e.dataTransfer.effectAllowed = 'move';
        e.dataTransfer.dropEffect = 'move';
        e.dataTransfer.setData('from-column-index', fromColumnIndex);
        e.dataTransfer.setData('type', 'column')
      },
      createTask(e, tasks) {
        this.$store.commit('CREATE_TASK', {
          tasks,
          name: e.target.value
        });
        e.target.value = ''
      }
    }
  }
</script>
<style lang="css">
	.column {
		@apply bg-grey-light p-2 mr-4 text-left shadow rounded;
		min-width: 350px;
	}
</style>
© 2020 GitHub, Inc.
