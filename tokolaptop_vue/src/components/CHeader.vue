<template>
	<!-- toolbar vuetify dengan warna primary -->
	<v-app-bar dark color="primary">

		<!-- header bagian kiri -->
		<!-- <v-app-bar-nav-icon @click="setSideBar(!sideBar)"></v-app-bar-nav-icon> -->
		<v-app-bar-nav-icon v-if="isHome" @click="setSideBar(!sideBar)"></v-app-bar-nav-icon>
		<v-btn v-if="!isHome" icon @click="$router.go(-1)">
			<v-icon>arrow_back</v-icon>
		</v-btn>

		<!-- header bagian tengah -->
		<v-toolbar-title class="white--text">Toko Laptop</v-toolbar-title>

		<!-- separator supaya header bagian kanan posisinya rata kanan -->
		<v-spacer></v-spacer>

		<!-- header bagian kanan -->
		<v-btn icon @click="cart()">
			<v-badge left overlap color="orange">
				<span slot="badge" v-if="countCart==0"> 0 </span>
				<span slot="badge" v-if="countCart>0"> {{ countCart }} </span>
				<v-icon>shopping_cart</v-icon>
			</v-badge>
		</v-btn>

		<!-- kolom pencarian dibawah header -->
		<v-text-field v-if="isHome" @click="search()" slot="extension" hide-details append-icon="mic" text label="Search" prepend-inner-icon="search" solo-inverted></v-text-field>

	</v-app-bar>
</template>
<script>
	import { mapGetters, mapActions } from 'vuex'
	export default {
		name: 'c-header',
		methods: {
			...mapActions({
				setSideBar : 'setSideBar',
				setStatusDialog: 'dialog/setStatus',
				setComponent: 'dialog/setComponent'
			}),
			search() {
				this.setStatusDialog(true)
				this.setComponent('search')
				this.setSideBar(false)
			},
			cart() {
				this.setStatusDialog(true)
				this.setComponent('cart')
				this.setSideBar(false)
			}
		},
		computed: {
			...mapGetters({
				sideBar : 'sideBar',
				countCart: 'cart/count'
			}),
			isHome () {
				return (this.$route.path==='/')
			},
		}
	}
</script>