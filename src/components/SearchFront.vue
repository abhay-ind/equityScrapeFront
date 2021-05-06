<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <div style="display:flex;place-content:center;">
      <input
        type="text"
        id="searchbar"
        class="form-control"
        v-model="search"
        @input="setsearch"
        placeholder="Search by Name"
      />
    </div>

    <div id="wholeFile">
      <a href="https://abhaya.pythonanywhere.com/downloadW/">
        <button class="btn btn-primary">Whole Day (CSV)</button>
      </a>
      <a :href="`http://abhaya.pythonanywhere.com/downloadC?search=${search}`">
        <button class="btn btn-secondary">Current Result (CSV)</button>
      </a>
      <div v-if="!loading" style="padding: 10px">
        <a style="font-weight:bold;font-size:20px;">{{ items.length }}</a> names
        found.
      </div>
    </div>
    <div style="place-content:center;display:flex;flex-direction:column">
      <div v-if="loading" style="font-weight:bold;font-size:20px;padding: 20px; font-color:blue; ">
        Loading...
      </div>
      <div
        v-if="!loading"
        style="display:flex;place-content:center;place-self:center;flex-direction:column;"
      >
        <ul style="margin-bottom:4px">
          <div
            style="display:flex;font-size:20px;margin:10px 0px 10px 0pxfont-weight:bold;background:#120E43;color:white;place-content:center;"
            class="list-group-item list-group-item-action"
          >
            <div id="sharename" style="color:orange">Name</div>
            <div id="sharename" style="color:orange">Code</div>
            <div id="sharename" style="color:lightblue">Open (₹)</div>
            <div id="sharename" style="color:lightblue">Close (₹)</div>
            <div id="sharename" style="color:lightgreen">High (₹)</div>
            <div id="sharename" style="color:red">Low (₹)</div>
          </div>
        </ul>
        <div
          style="overflow:scroll;overflow-x:hidden;height:50vh;border-bottom-left-radius:10px;border-bottom-right-radius:10px;"
        >
          <ul>
            <li
              v-for="item in items"
              v-bind:key="item.name"
              style="display:flex;flex-direction: column;align-content:center;justify-content:center;padding-bottom:10px;place-self:center;margin:10px 10px 2px 5px"
              class="list-group-item list-group-item-action"
            >
              <div style="display:flex;place-content:center;">
                <div id="sharename" style="font-weight: bolder !important; ">
                  {{ item.name }}
                </div>
                <div id="sharename">{{ item.code }}</div>
                <div id="sharename">{{ item.open }}</div>
                <div id="sharename">{{ item.close }}</div>
                <div id="sharename">{{ item.high }}</div>
                <div id="sharename">{{ item.low }}</div>
              </div>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  data() {
    return {
      noresult: false,
      loading: true,
      search: "",
      items: [],
    };
  },
  beforeMount() {
    this.loading = true;
    fetch("https://abhaya.pythonanywhere.com/search/?search=*")
      .then((response) => response.json())
      .then((data) => {
        console.log(data.result);
        this.items = data.result;
        this.noresult = false;
        this.loading = false;
      });
  },
  methods: {
    created() {},
    toggleNoResults() {
      this.noresult = !this.noresult;
    },
    setsearch: function(event) {
      this.search = event.target.value;
      var key="";
      if (this.search === "") {
        key="*";
      }
      else
      key=this.search
      fetch("https://abhaya.pythonanywhere.com/search/?search=" + key)
        .then((response) => response.json())
        .then((data) => {
          console.log(data.result);
          this.items = data.result;
          this.noresult = false;
        });
    },
  },
  props: {
    msg: String,
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
::-webkit-scrollbar {
  width: 6px;
}

/* Track */
::-webkit-scrollbar-track {
  background: white;
  border-radius: 4px;
}

/* Handle */
::-webkit-scrollbar-thumb {
  background: gray;
  border-radius: 4px;
}

/* Handle on hover */
::-webkit-scrollbar-thumb:hover {
  background: #5daffa;
}
.btn {
  margin: 10px !important;
}
#sharename {
  text-align: center;
  width: 10vw;
  margin: 5px;
  overflow: hidden;
  text-overflow: ellipsis;
}

#date {
  padding: 7px;
  margin: 4px;
}

#wholefile {
  display: flex;
}

#searchbar :hover {
  background: #42b983;
}
#searchbar {
  width: 40vw;
  padding: 10px;
}
h3 {
  margin: 0 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
