<template>
	<div id="navigation">
		<div class="hamIcon"><svg @click='toggleNav()' class='hamSvg' height="32px" id="Layer_1" style="enable-background:new 0 0 32 32;" version="1.1" viewBox="0 0 32 32" width="32px" xml:space="preserve" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink"><path d="M4,10h24c1.104,0,2-0.896,2-2s-0.896-2-2-2H4C2.896,6,2,6.896,2,8S2.896,10,4,10z M28,14H4c-1.104,0-2,0.896-2,2  s0.896,2,2,2h24c1.104,0,2-0.896,2-2S29.104,14,28,14z M28,22H4c-1.104,0-2,0.896-2,2s0.896,2,2,2h24c1.104,0,2-0.896,2-2  S29.104,22,28,22z"/></svg></div>
		
		<transition name='fade' mode='out-in'>
			<div id="navigationWrapper" class='maxIs' v-if='checkNavigation'>
				<div class="left">
					<ul>

						<li><a href="/accounts/login">ورود</a></li>
						<li><a href="/accounts/signup">ثبت نام</a></li>
						<li><a href="/users/profile">پنل کاربری</a></li>

					</ul>
				</div>
				<div ref='allCategories' id="allCategories">
					<ul>
						<li @click='toggleSubMenu();checkTop($event)'>
							دسته بندی محصولات
						</li>
					</ul>
				</div>


				
				<div class="right">
					<ul>
						<li><a href="/">خانه</a></li>
						<li><a href="#">درباره ما</a></li>
						<li><a href="/blog/posts/">وبلاگ</a></li>
						
						
					</ul>

				</div>
			

			</div>
		</transition>
		<!-- <transition name='toggleSubMenu' mode='out-in'> -->
			<flat-menu v-if='checkSubMenuIsOpen'></flat-menu>
		<!-- </transition> -->
	</div>


</template>

<script>
	import ham from "../hamIcon/ham.vue"
	import slicer from "../slicer/slicer.vue"
	import flatMenu from "../flatMenu/flatMenu.vue"
	import {mapActions} from "vuex"

	export default{
		name:"navigation",
		methods:{
			...mapActions([
			'toggleSubMenu'
			]),
			openCats(){
				console.log("hey")
				const flatMenu=document.querySelector("#flatMenu")
				flatMenu.style.display='block'
			},
			toggleNav(){
				this.checkNavigation=!this.checkNavigation
			},
			checkNavigationMethod(){
				if(window.innerWidth>801)
				{
					console.log("ham")
					const ham=document.querySelector(".hamSvg")
					ham.style.display='none'
					
					this.checkNavigation=true
					return
				}
				else{
					const ham=document.querySelector(".hamSvg")
					ham.style.display='block'
					this.checkNavigation=false

				}
			},
			checkTop(e)
			{
				const el=e.target
				const bond=el.getBoundingClientRect()
				this.$store.state.categoriesTop=bond.top+5+bond.height + window.scrollY+'px'
				console.log(bond)

			},
			closeSubMenuIfOpen(){
				if(this.$store.state.isSubMenuOpen){
					if(window.innerWidth>924)
					{
						this.toggleSubMenu()
					}
				}
			}
			
			
		},
		computed:{
			checkSubMenuIsOpen(){
				return this.$store.state.isSubMenuOpen
			}
		},
		mounted(){
			if(window.innerWidth>801)
			{
				this.checkNavigation=true
			}
			window.addEventListener("resize",this.checkNavigationMethod)
			window.addEventListener("resize",this.closeSubMenuIfOpen)
		},
		data(){
			return{
				showSubMenu:true,
				checkNavigation:false
			}
		},
		components:{
			ham,
			flatMenu
		}
	}
</script>
<style scoped>
	.hamIcon{
		display:none;
		background: #f6f6f4;
		widtH:100%;
		justify-content:center;
		border-top:1px solid #d9d8d8
	}
	#navigation{
		display: flex;
		justify-content: center;
		width:100%;
		background: #f6f6f4;
		border-bottom:1px solid #d9d8d8
		
	}
	a{
		color:black;
		padding:5px
	}
	.hamSvg{
		margin:10px 0 10px 0;
	}

	#navigationWrapper{
		padding: 10px;
		width: 100%;
		display: flex;
		justify-content: space-between;
		z-index:667;
		background: #f6f6f4;
	}
	ul{
		display: flex;
	}
	ul li:not(:first-child),ul li:not(:first-child)
	{
		border-left:1px solid white
	}
	#allCategories{
		color:black;
		display: none;
	}
	#allCategories ul li{
		position: relative;
	}
	@media (max-width: 801px){
		#navigation{
			display: flex;
			flex-direction: column;
			align-items: center;
		}
		.hamIcon{
			display:flex
		}
		.right,.left{
			width:100%;
			flex-direction: column;
			align-items: center;
			justify-content: center;
		}
		.right ul li,.left ul li{
			padding-right:0
		}
		#navigationWrapper{
			flex-direction: column;
			align-items: center;
			justify-content: center;
			position: absolute;
			top:305px;
			z-index:667
		}
		ul{
			flex-direction: column;
			align-items: center;
			width:100%
		}
		li{
			padding-right: 0;
			margin-top: 10px;
			text-align: center;
		}
		ul li:not(:first-child),ul li:not(:first-child)
		{
			border-left:0
		}
		#allCategories{
			order:3;
			display:block
		}
		svg{
			fill:rgb(11,111,210)
		}
	}

	.fade-enter-active{
		animation: fadeIn 0.2s linear;
	}
	.fade-leave-active{
		animation: fadeOut 0.2s linear;
	}
	.toggleSubMenu-enter-active{
		animation:ToggleSubMenuIn 0.3s linear;
	}
	.toggleSubMenu-leave-active{
		animation:ToggleSubMenuOut 0.3s linear;
	}
	@keyframes ToggleSubMenuIn {
		from{
			opacity:0;
			top:0
		}
		to{
			opacity:1;
		}
	}
	@keyframes ToggleSubMenuOut {
		from{
			opacity:1;
		}
		to{
			opacity:0;	
		}
	}
	@keyframes fadeIn {
		from{
			transform: translateY(-100%);
			opacity: 0;
		}
		to{
			transform: translateY(0);
			opacity: 1;
		}
	}
	@keyframes fadeOut {
		from{
			transform: translateY(0);
			opacity: 1;
		}
		to{
			transform: translateY(-100%);
			opacity: 0;
		}
	}
</style>