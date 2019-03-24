<template>
	<li>
		<ul>
			<li class="todo-list__pin"><i class="fas fa-map-pin"></i></li>
			<!-- delModeの変更をscript内で処理するにはカスタムディレクティブが
				必要となるが、クラスのtoggle程度の処理なので直接bindで定義
			 -->
			<li v-bind:class=
				"{ 'todo-list__item': true,
					'todo--mode-done': !delMode,
					'todo--mode-del': delMode}"				
				>
				<slot></slot>
			</li>
		</ul>	
	</li>		
</template>
<script>
	export default {
		props: ['todoList', 'delMode']		
	}
</script>


<style lang="scss">
	.todo {
		//完了モード
		&--mode-done {
			&:hover {
				background-color: #fff45e;
				opacity: 0.4;
				cursor: pointer;
			}
		}
		
		//削除モード
		&--mode-del {
			&:hover {
				background-color: #f44271;
				opacity: 0.4;
				cursor: pointer;
			}
		}
	}
</style>