<template>
	<div>
		<!-- Bagian pertama yaitu Category -->
		<v-container grid-list-md>
			<v-subheader>
				Kategori
				<v-spacer></v-spacer>
				<!-- link ke router categories yang nantinya akan kita definisikan routing dan componentnya -->
				<router-link to="/categories">Lihat Semua</router-link>
			</v-subheader>
			<v-layout row wrap>
				<!-- variabel categories ini nantinya kita isi dengan data dari endpoint category -->
				<v-flex v-for="category in categories" xs6 :key="category.id">
					<v-card :to="'/category/'+ category.slug">
						<!-- untuk load image supaya lebih rapi akan kita buatkan method getImage -->
						<v-img :src="getImage('/categories/'+category.image)" height="150px">
							<v-container fill-height fluid pa-2>
								<v-layout fill-height>
									<v-flex xs12 align-end flexbox>
										<!-- nama category-nya akan ditampilkan disini -->
										<span class="title white--text text-block" v-text="category.name"></span>
									</v-flex>
								</v-layout>
							</v-container>
						</v-img>

						<!-- icon dummy saja, nantinya kamu bisa sesuaikan -->
						<v-card-actions>
							<v-spacer></v-spacer>
							<v-btn icon>
								<v-icon>favorite</v-icon>
							</v-btn>
							<v-btn icon>
								<v-icon>bookmark</v-icon>
							</v-btn>
							<v-btn icon>
								<v-icon>share</v-icon>
							</v-btn>
						</v-card-actions>
					</v-card>
				</v-flex>
			</v-layout>
		</v-container>

	<!-- Bagian kedua yaitu book -->
	<v-container grid-list-md>
		<v-subheader>
		Produk Laptop Terpopuler
			<v-spacer></v-spacer>
			<!-- link ke route books yang nantinya akan kita definisikan routing dan componentnya -->
			<router-link to="/books">Lihat Semua</router-link>
		</v-subheader>
		<v-layout row wrap>
			<!-- data buku kita tampilkan dalam dua kolom (xs6) -->
			<v-flex v-for="(book, index) in books" xs6 :key="index">
				<v-card :to="'/book/'+ book.slug">
					<v-img :src="getImage('/books/'+book.cover)" height="150px">
						<v-container fill-height fluid pa-2>
							<v-layout fill-height>
								<v-flex xs12 align-end flexbox>
									<span class="title white--text text-block" v-text="book.title"></span>
								</v-flex>
							</v-layout>
						</v-container>
					</v-img>
					<v-card-actions>
						<v-spacer></v-spacer>
						<v-btn icon>
							<v-icon>favorite</v-icon>
						</v-btn>
						<v-btn icon>
							<v-icon>bookmark</v-icon>
						</v-btn>
						<v-btn icon>
							<v-icon>share</v-icon>
						</v-btn>
					</v-card-actions>
				</v-card>
			</v-flex>
		</v-layout>
	</v-container>
	</div>
</template>

<style scoped>
	/* mengatur posisi judul */
	.text-block {
		position: absolute;
		bottom: 5px;
		left: 5px;
		background-color: black;
		padding-left: 5px;
		padding-right: 5px;
		opacity: 0.7;
		width: 100%;
	}
</style>

<script>
	export default {
		data: () => ({
			categories: [], // kita definisikan sebagai array kosong
			books: []
		}),
		methods: {
			getImage (image){
				if(image!=null && image.length>0){
					return process.env.VUE_APP_BACKEND_URL + "/images"+ image
				}
				// default image jika tidak ditemukan,
				// letakkan image ini pada folder /public/img di project vue
				return "/img/unavailable.jpg"
			},
		},
		created(){
			let count = 4
			// request ke endpoint category random dengan parameter count = 4
			this.axios.get('/categories/random/'+count)
			.then((response) => {
				let categories = response.data.data
				//ketika dapat datanya maka nilainya dimasukkan ke dalam properti data categories
				this.categories = categories
			})
			.catch((error) => {
				let responses = error.response
				console.log(responses)
			})
			count = 8
			//request ke endpoint top book dengan parameter count = 8
			this.axios.get('/books/top/'+count)
			.then((response) => {
				let books = response.data.data
				this.books = books
			})
			.catch((error) => {
				let responses = error.response
				console.log(responses)
			})
		}
	}
</script>