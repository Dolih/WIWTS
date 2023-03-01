<template>
  <div>
    <label>Введите жанр:</label>
    <input type="text" v-model="genre">
    <label>Введите минимальный рейтинг (опционально):</label>
    <input type="text" v-model="rating">
    <button @click="search">Поиск</button>

    <div v-if="loading">Загрузка...</div>
    <div v-else-if="film">
      <h2>Найденный фильм:</h2>
      <p>{{ film.name_russian }} {{ film.age_restriction }}+</p>
      <h2>Описание:</h2>
      <p>{{ film.description }}</p>
      <h2>Оценка:</h2>
      <p>{{ film.rating_kp }}</p>
      <img :src=film.small_poster>
    </div>
    <div v-else>Нет фильмов с таким жанром на этой странице. Попробуйте еще раз.</div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      genre: '',
      rating: '',
      film: null,
      loading: false,
    };
  },
  methods: {
    async search() {
  this.loading = true;

  while (true) {
    const page = Math.floor(Math.random() * 10000) + 1;
    const url = `https://kinobd.ru/api/films?page=${page}`;
    const response = await fetch(url);
    const data = await response.json();

    const filteredFilms = data.data.filter(film =>
      film.genres.some(genre => genre.name_ru.toLowerCase().includes(this.genre.toLowerCase()))
    ).filter(film => film.rating_kp_count > 1000); 

    if (filteredFilms.length > 0) {
      let randomIndex = Math.floor(Math.random() * filteredFilms.length);
      let selectedFilm = filteredFilms[randomIndex];

      if (this.rating && selectedFilm.rating_kp < parseInt(this.rating)) {
        continue;
      }

      this.film = selectedFilm;
      break;
    }
  }

  this.loading = false;
},
  },
};
</script>





<style>

</style>


<style>

</style>
