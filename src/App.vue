<template>
    <div class="container">
    	<app-title></app-title>
    	
    	<!-- event -[要素追加, モード切り替え]
    			props -[モード, Todoリストの状態, リストの長さ]
    		-->
    	<app-header 
			v-on:todo-added="createNewRoutine"
			v-on:mode-changed="toggleMode"
			v-bind:delMode="delMode"
			v-bind:routineStatus="routineStatus"
			v-bind:routineCount="todoList.length"
			>
			</app-header>
   	
    	<!-- event -[要素クリック]
    			props -[Todoリスト, モード]
    		-->
    	<app-todo-list 
    		v-on:todo-clicked="popRoutine"
    		v-bind:todoList="todoList"
    		v-bind:delMode="delMode"
    		>
    		</app-todo-list>
	</div>
</template>

<script>
	//import処理
	import Title from "./components/Title.vue";
	import Header from "./components/Header.vue"; 
	import TodoList from "./components/TodoList.vue";
	
	export default {
		data: function() {
			return {
				//              連番       完了数    残数
				routineStatus: {serial: 8, done: 0, rest: 8},
				//今回は初期値として手入力でセット
				//SpringBootと組み合わせるときにDBから取ったりできるようにしたい
				todoList: [
					{id: 0, value: 'グリームニルをしばく'},
					{id: 1, value: 'マグナ3回×6属性'},
					{id: 2, value: 'ティアマトマグナHL2回'},
					{id: 3, value: '島HARD6属性'},
					{id: 4, value: 'たのしいヘイロー'},
					{id: 5, value: '天司の試練+天司マルチ'},
					{id: 6, value: '古代布2回'},
					{id: 7, value: 'アーカルムパスポート消化'}
				],
				//true...削除モード false...削除モード
				delMode: false
			};
		},
		
		//コンポーネント定義
		components: {
			appTitle: Title,
			appHeader: Header,
			appTodoList: TodoList
		},
		
		methods: {
			/**
			 * 入力値で新規登録
			 * @param {String} newRoutine -フォームの入力値 改行コードは要変換
			 */
			createNewRoutine: function(newRoutine) {
				//改行コードはスペースとなってしまうのでHTMLのbrタグへ変換
				this.todoList.unshift(
					{id: this.routineStatus.index , 
					 value: newRoutine.replace(/\r?\n/g, '<br>')
					});
				
				this.routineStatus.serial++;
				this.routineStatus.rest++;
			},
			
			/**
			 * リストから要素を削除する
			 * 完了モード→doneを更新して削除 削除モード→doneを更新せずに削除
			 * @param {Number} index -削除対象のインデックス
			 */
			popRoutine: function(index) {
				this.todoList.splice(index, 1);
				if (this.delMode) {
					this.routineStatus.rest--;
				}
				if (!this.delMode) {
					this.routineStatus.rest--;
					this.routineStatus.done++;
				}
				
			},
			
			/**
			 * ボタン押下によって各要素クリック時の挙動を切り替える
			 * delMode→削除 !delMode→完了
			 */
			toggleMode: function() {
				this.delMode = !this.delMode;
			}
		}
}
</script>

<style lang="scss">
		
</style>
