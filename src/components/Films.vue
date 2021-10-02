<template>
  <div>
    <h1>Search via Title</h1>
    <div class="search">
      <input
        v-model="data.searchString"
        type="text"
        class="searchInput"
        placeholder="Type Here"
      />
      <button @click="getData" type="submit" class="searchButton">
        Search
      </button>
      <input
        v-model="data.filterString"
        type="text"
        class="filterInput"
        placeholder="Filter list via Title"
      />
    </div>
    <div class="search" style="margin-top: 20px"></div>
    <div class="panel">
      <div class="container">
        <div v-if="data.results.length">
          <div class="card" v-for="(d, index) in filterData" :key="index">
            <div v-if="d.Response === 'True'">
              <div style="margin-top: 5px">
                <strong>{{ d.Title }}</strong>
              </div>
              <div>
                <img :src="d.Poster" />
              </div>
              <div style="margin-top: 5px">
                <strong>Director:</strong> {{ d.Director }}
              </div>
              <div><strong>Writer:</strong> {{ d.Writer }}</div>
              <div><strong>Actor:</strong> {{ d.Actors }}</div>
              <div><strong>Release Date:</strong> {{ d.Released }}</div>
              <div class="viewtoggle">
                <a @click="activateReadMore(d)" v-if="!d.readMore"
                  >View more details</a
                >
              </div>
              <div v-if="d.readMore">
                <div><strong>Language:</strong> {{ d.Language }}</div>
                <div><strong>Runtime:</strong> {{ d.Runtime }}</div>
                <div><strong>Genre:</strong> {{ d.Genre }}</div>
                <div><strong>Country:</strong> {{ d.Country }}</div>
                <div class="viewtoggle">
                  <a @click="activateReadMore(d)">View less details</a>
                </div>
              </div>
            </div>
          </div>
        </div>
        <div v-else>
          <h2>No data available.</h2>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: "Films",
  data() {
    return {
      data: {
        searchString: "",
        results: [],
        filterString: "",
      },
    };
  },
  computed: {
    filterData: function () {
      var search_array = this.data.results,
        searchString = this.data.filterString;
      searchString = searchString.trim().toLowerCase();
      if (!searchString) {
        return search_array;
      }
      search_array = search_array.filter((i) => {
        if (i.Title.toLowerCase().indexOf(searchString) !== -1) {
          return i;
        }
      });
      return search_array;
    },
  },
  methods: {
    activateReadMore(d) {
      d.readMore = !d.readMore;
    },
    async getData() {
      this.data.results = [];
      let imdbids = this.data.searchString.split(",");
      for (let id of imdbids) {
        if (id.trim() !== "") {
          const response = await fetch(
            `https://www.omdbapi.com/?t=${id}&apikey=fc5d7c68`
          );
          const res = await response.json();
          res.readMore = false;
          res.favourite = false;
          this.data.results.push(res);
          this.data.reserveData.push(res);
        }
      }
    },
  },
};
</script>
<style>
.card {
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
  transition: 0.3s;
  margin: 10px;
  float: left;
  width: 45%;
}
.card:hover {
  box-shadow: 0 8px 16px 0 rgba(0, 0, 0, 0.2);
}
.container {
  padding: 2px 16px;
}
.panel {
  display: inline-block;
  overflow: hidden;
  width: 100%;
  margin-top: 20px;
}
img {
  float: left;
  height: 320px;
  width: 160px;
  margin-right: 20px;
}
@import url(https://fonts.googleapis.com/css?family=Open+Sans);
body {
  background: #f2f2f2;
  font-family: "Open Sans", sans-serif;
}
.search {
  width: 100%;
  position: relative;
  display: flex;
}
.searchTerm {
  width: 100%;
  border: 3px solid #00B4CC;
  border-right: none;
  padding: 5px;
  height: 20px;
  border-radius: 5px 0 0 5px;
  outline: none;
  color: #9DBFAF;
}
.searchTerm:focus {
  color: #00B4CC;
}
.searchButton {
  width: 75px;
  height: 36px;
  border: 1px solid #00B4CC;
  background: #00B4CC;
  text-align: center;
  color: #fff;
  border-radius: 0 5px 5px 0;
  cursor: pointer;
  font-size: 20px;
}
.wrap {
  width: 30%;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
.searchInput {
  width: 450px;
}
.filterInput {
  width: 450px;
  float: right;
  margin-left: 100px;
}
.viewtoggle {
  margin-top: 20px;
}
</style>