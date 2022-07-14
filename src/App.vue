<template>
  <div id="app" class="container">
    <HomePage :folder_structure=folder_structure></HomePage>
  </div>
</template>

<script>
import HomePage from './components/HomePage.vue'
import JsonFile from './assets/file.json'

export default {
  name: 'App',
  components: {
    HomePage
  },
  data(){
    return{
      my_json: JsonFile,
      folder_structure:[]
    }
  },
  mounted() {
    this.getFileFolderStructure();
  },
  methods: {
    getFileFolderStructure() {
      console.log(this.my_json[0]);
      var my_json_count = this.my_json.length;
      var folder_arr=[];
      for(var i = 0; i < my_json_count; i++) {
        folder_arr.push(this.my_json[i]['path']);
      }
      this.addFilesFolders(folder_arr);
    },
    addFilesFolders(folder_arr) {
      let result = [];
      let level = {result};

      folder_arr.forEach(path => {
        path.split('/').reduce((r, name) => {
          if(!r[name]) {
            r[name] = {result: []};
            var url = '', size = '';
            if(name.includes('.')) {
              var data = this.checkFileDetails(name);
              url = data.url;
              size = data.size;
            }
            r.result.push({name, children: r[name].result, url, size, checked: false});
          }
          
          return r[name];
        }, level)
      });
      // console.log(result);
      this.folder_structure = result;
    },
    checkFileDetails(name) {
      var data = {};
      this.my_json.forEach(element => {
        if(element['path'].includes(name)) {
          data = {'url': element['url'], 'size': element['size']};
        }
      });
      return data;
    }
  }

}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
