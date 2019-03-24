<template>
	<div class="header">
		<ul class="header__menu-list">
			<!-- キャラクター 完了か否かで表示切り替え -->
			<li v-show="!completed" class="menu__item">
				<img src="../assets/normal.png" class="menu__item--icon">
			</li>
			<li v-show="completed" class="menu__item">
				<img src="../assets/done.png" class="menu__item--icon">
			</li>
			
			<!-- 入力エリア -->
			<li class="menu__item">
				<textarea v-model="newRoutine" class="menu__item--input"></textarea>
			</li>
			
			<!-- 状態 -->
			<li class="menu__item">
				<ul class="menu__item--action">
					<li>やること: &nbsp;&nbsp; {{ routineCount }}</li>
					<li>できた: &nbsp;&nbsp; {{ routineStatus.done }}</li>
					<li>まだ: &nbsp;&nbsp; {{ routineStatus.rest }}</li>
				</ul>
			</li>
		</ul>
		
		<ul class="header__menu-button-list">
			<!-- アクションボタン -->
			<li class="menu__item">
				<button v-on:click="addNew" class="button--add">日課を増やす</button>
			</li>
			<li v-show="!delMode" class="menu__item">
				<button v-on:click="toggleMode" class="button--del">勝利を信じて</button>
			</li>
			<li v-show="delMode" class="menu__item">
				<button v-on:click="toggleMode" class="button--retry">復活する</button>
			</li>
		</ul>
		
    	
	</div>
</template>


<script>
	export default {
		props: ['delMode', 'routineStatus', 'routineCount'],
		data: function() {
			return {
				newRoutine: ''
			}
		},
		
		//v-showディレクティブ内ではメソッドを呼び出せないのでcomputedで処理
		computed: {
			/**
			 * 日課が全て完了したか判定
			 * @returns {Boolean} -true→完了 false→未完了
			 */
			completed: function() {
				return this.routineStatus.rest === 0 ? true : false;
			}	
		},
		
		methods: {
			/**
			 * 要素を追加 親要素へemit
			 */
			addNew: function() {
				this.$emit('todo-added', this.newRoutine);
				this.newRoutine = '';
			},
			/**
			 * モード切り替え 親要素へemit
			 */
			toggleMode: function() {
				this.$emit('mode-changed');
			},
				
		}		
	}
</script>


<style lang="scss">
	//レイアウト
	.header {
		width: 80%;
		margin: 30px auto;
		
		&__menu-list {
			@include flex-center;
			width: 80%;
		}
		
		&__menu-button-list {
			@include flex-right;
			width: 50%;
			margin-left: 250px;
		}
	}
	
	//メニュー要素
	.menu__item {
		
		margin-right: 15px;
		
		&--input {
			background-color: #ecf0f1;
			width: 300px;
			height: 200px;
			font-size: 24px;
			border-radius: 10px;
			box-shadow: 0;
		}
		
		&--action {
			margin-top: 50px;
			font-size: 24px;
			line-height: 55px;
		}
	}
	
	//ボタン要素
	.button--add {
		@extend %button;
		background-color: #b2f0ff;
		color: #fff;
		
		width: 150px;
		height: 50px;
	}
	
	.button--del {
		@extend %button;
		background-color: #f44271;
		color: #fff;
		
		width: 150px;
		height: 50px;
	}
	
	.button--retry {
		@extend %button;
		background-color: #fff45e;
		color: #fff;
		
		width: 150px;
		height: 50px;
	}
</style>