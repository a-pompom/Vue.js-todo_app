<template>
	<div class="todo">
		<ul class="todo-list">
			<!-- event -[要素クリック(要素id)]
    			props -[モード]
    		-->
			<item
				v-on:click.native="todoClicked(index)"
				v-for="(todo, index) in todoList"
				v-bind:key="todo.id"				
				v-bind:delMode="delMode"
				
				>
				<span v-html="todo.value"></span>
			</item>
			
			
		</ul>
	</div>
</template>


<script>
	import Todo from "./Todo.vue";
	export default {
		props: ['todoList', 'delMode'],
		
		components: {
			Item : Todo
		},
		
		methods: {
			/**
			 * 各要素クリック時に呼ばれる処理
			 * @param {Number} index -クリック要素を判別するインデックス
			 */
			todoClicked: function(index) {
				this.$emit('todo-clicked', index);
			}
		}
		
	}
</script>


<style lang="scss">
	//リスト全体
	.todo-list {
		@include flex-table;
		align-items: center;
		width: 100%;
		margin: 10px 10px 10px 50px;
		
		//リスト要素
		&__item {
			box-sizing: border-box;
			width: 300px;
			height: 200px;
			background-color: #f9f9f9;
			font-size: 20px;
			border-radius: 10px;
			padding: 50px 30px;
			margin: 20px 20px;
		}
		
		// 各要素の上部にピンをつける
		&__pin {
			text-align: center;
			font-size: 20px;
		}
		
	}
</style>