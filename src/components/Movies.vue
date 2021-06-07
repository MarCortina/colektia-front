<template>
  <div>
  <h1>{{ msg }}</h1>
    <nav class="navbar navbar-light bg-light bg-dark">
      <a href="/" class="navbar-brand text-white"> Refresh </a>
    </nav>

    <div class="container">
      <div class="row pt-5">
        <div class="col-md-5">
          <div class="card">
            <div class="card-body">
              <form @submit.prevent="addMovie">
                <div class="form-group">
                  <input
                    type="text"
                    v-model="movie.title"
                    placeholder="Title"
                    class="form-control"
                  />
                </div>

                <div class="form-group">
                  <textarea
                    v-model="movie.description"
                    cols="30"
                    rows="10"
                    class="form-control"
                    placeholder="Description"
                  ></textarea>
                </div>
                <template v-if="edit === false">
                  <button class="btn btn-primary btn-block">Send</button>
                </template>
                <template v-else>
                  <button class="btn btn-primary btn-block">Update</button>
                </template>
              </form>
            </div>
          </div>
        </div>

        <div class="col-md-7">
          <table class="table table-bordered">
            <thead>
              <tr>
                <th>MOVIE</th>
                <th>DESCRIPTION</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="movie of movies" :key="movie">
                <td>{{ movie.title }}</td>
                <td>{{ movie.description }}</td>
                <td>
                  <button @click="delete movie._id" class="btn btn-danger">
                    DELETE
                  </button>
                  <button @click="editMovie(movie._id)" class="btn-secondary">
                    EDIT
                  </button>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
class Movie {
  constructor(title, description) {
    this.title = title;
    this.description = description;
  }
}

export default {
  data() {
    return {
      movie: new Movie(),
      movies: [],
      edit: false,
      toEdit: "",
    };
  },
  created() {
    this.getMovie();
  },
  name: "Movies",
  props: {
    msg: String,
  },
  methods: {
    getMovie() {
      fetch("/")
        .then((res) => res.json())
        .then((data) => {
          this.movies = data;
        });
    },
    addMovie() {
      if (this.edit === false) {
        fetch("/api", {
          method: "POST",
          body: JSON.stringify(this.movie),
          headers: {
            Accept: "application/json",
            "Content-type": "application/json",
          },
        })
          .then((res) => res.json())
          .then((data) => {
            this.getMovie(data);
          });
      } else {
        fetch("/api" + this.toEdit, {
          method: "PUT",
          body: JSON.stringify(this.movie),
          headers: {
            Accept: "application/json",
            "Content-type": "application/json",
          },
        })
          .then((res) => res.json())
          .then((data) => {
            this.getMovie(data);
            this.edit = false;
          });
      }

      this.movie = new Movie();
    },
    delete(id) {
      fetch("/api" + id, {
        method: "DELETE",
        headers: {
          Accept: "application/json",
          "Content-type": "application/json",
        },
      })
        .then((res) => res.json())
        .then((data) => {
          this.getMovie(data);
        });
    },
    editMovie(id) {
      fetch("/api" + id)
        .then((res) => res.json())
        .then((data) => {
          this.movie = new Movie(data.title, data.description);
          this.toEdit = data._id;
          this.edit = true;
        });
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
