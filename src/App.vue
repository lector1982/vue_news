<template>
<div class="container2">

	<div class="row">
		<div class="input-field col s3 offset-s2">
			<input placeholder="Search..." type="text" v-model="search" @keypress.enter="searchNews">
  </div>

	<div class="input-field col s3">
			<select v-model="country" @change="searchNews">
				<option value="" disabled selected>Choose country</option>
				<option value="ua">Ukraine</option>
				<option value="ru">Russia</option>
				<option value="pl">Poland</option>
				<option value="ie">Ireland</option>
				<option value="us">USA</option>
			</select>
  </div>

	<div class="input-field col s2">
		<button class="waves-effect waves-light btn" @click="searchNews">Search</button>
	</div>
</div>

<div class="row" v-show="notFound">
	News not found
</div>

<div class="preloader-wrapper big active" v-if="isLoad">
    <div class="spinner-layer spinner-blue-only">
      <div class="circle-clipper left">
        <div class="circle"></div>
      </div><div class="gap-patch">
        <div class="circle"></div>
      </div><div class="circle-clipper right">
        <div class="circle"></div>
      </div>
    </div>
  </div>

<div class="row">
    <div class="col s12 m4" v-for="(article,index) in news" :key="index">
      <div class="card card-small">
        <div class="card-image">
          <img :src="article.urlToImage" :alt="article.title">
        </div>
        <div class="card-content">
					<span class="card-title">{{article.title}}</span>
          <p>{{article.description}}</p>
        </div>
        <div class="card-action">
          <a href="{{article.url}}">More</a>
        </div>
      </div>
    </div>
  </div>
</div>

</template>

<script>
import M from 'materialize-css'
import axios from 'axios'

export default {
  name: 'App',
	data() {
		return {
			news: [],
			search: '',
			isLoad: false,
			country: 'us',
			notFound: false
		}
	},
	mounted () {
    M.AutoInit();
	},
  methods: {
		searchNews() {
			this.isLoad = true;
			axios.get(`https://newsapi.org/v2/top-headlines?country=${this.country}&q=${this.search}&sortBy=publishedAt&apiKey=07e2467be2a84a05ac9f3d4c43018995`)
			.then(response => {
				console.log(response.data);
				if(response.data.status == 'ok') {
					this.news = response.data.articles;
					this.isLoad = false;
				}
				if(response.data.status == 'ok' && response.data.totalResults == 0) {
					this.notFound = true;
				}
			})
		}
	}
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.card .card-image {
	height: 200px;
	overflow: hidden;
}
.card .card-image img {
	height: 100%;
	object-fit: cover;
	object-position: top;
}
.card .card-content {
	padding: 15px;
}
.card .card-content .card-title {
	white-space: nowrap;
	overflow: hidden;
	text-overflow: ellipsis;
}
.card .card-content p {
	text-overflow: ellipsis;
  overflow: hidden;
  display: -webkit-box;
	-webkit-line-clamp: 3;
	-webkit-box-orient: vertical;
	min-height: 66px;
}
</style>
