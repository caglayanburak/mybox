<template>
  <div class="hello container">
    <h1>{{ msg }}</h1>
 
    <h2>Essential Links</h2>
    <b-btn v-b-modal.modal1>Yeni Kutu</b-btn>
        <div class="row col-md-12">
<div class="col-md-4" v-for="(item,index) in items">
  <b-jumbotron >
  <h4>{{item.boxName}}</h4>
  <small>{{item.id}}</small>
  <b-list-group>
  <b-list-group-item v-for="(user,index2) in item.users">
      {{user.name}} 
  </b-list-group-item>
 
</b-list-group>

 <div>
    <h5>Katılımcı Sayısı:{{item.users.length}}</h5>
  
  </div>
</b-jumbotron>
</div>
</div>
     <div v-if="newBox">
     <div>
 
     </div>
     <div>
  </div>
     
  </div>
 <div>
       
     
        <!-- Modal Component -->
        <b-modal id="modal1"
                 ref="modal1"
                 title="Submit your name"
                 @ok="handleOk"
                 @shown="clearName">
       
              <b-form class="col-md-12  ml-auto  mr-auto" @submit.stop.prevent="handleSubmit">
      <b-form-group id="exampleInputGroup1"
                    label="Kutu Adı:" label-for="exampleInput1">
        <b-form-input id="exampleInput1"
                      type="text" v-model="boxName"
                      placeholder="Kutu Adı"
        ></b-form-input>
      </b-form-group>
      <b-form-group id="exampleInputGroup2"
                    label="Ad-Soyad:" label-for="exampleInput2">
        <b-form-input id="exampleInput2"
                      type="text" v-model="name"
                      placeholder="ad-soyad"
        ></b-form-input>
      </b-form-group>
        <b-form-group id="exampleInputGroup3"
                    label="Email:" label-for="exampleInput3">
        <b-form-input id="exampleInput2"
                      type="text" v-model="email"
                      placeholder="email"
        ></b-form-input>
      </b-form-group>
      <b-form-group id="exampleInputGroup2"
                    label="Açıklama:" label-for="exampleInput2">
        <b-form-input id="exampleInput2"
                      type="text" v-model="description"
                      placeholder="Açıklama"
        ></b-form-input>
      </b-form-group>
    </b-form>
        </b-modal>
    </div>
  </div>
 

</template>

<script>

export default {
  name: 'hello',
  data () {
    return {
      name:'',
      names:[],
      msg: 'Welcome to Your Vue.js App',
      message:'hello world',
      newBox:false,

       items: []
    }
  },
  
   mounted() {
     var vm=this;
                firebase.database().ref('boxes').on('value',snap => 
                this.items=snap.val()
                
                //vm.items.push(snap.val())
                //console.log(snap.val())
                );
                
        },
  methods:{
    myalert:function(){
      console.log('test');
    },
    show :function() {
      this.newBox=true;
  },
    clearName() {
                this.name = '';
            },
            handleOk(e) {
                e.cancel();
                // if (!this.name) {
                //     alert('Please enter your name');
                // } else {
                //     this.handleSubmit()
                // }
                this.saveBox();
            },
            handleSubmit() {
                this.names.push(this.name);
                this.clearName();
                this.$refs.modal1.hide()
            },
  updateBoxUser:function (email) {
  // A post entry.
  var postData = {
    name: name,
    email: email,
    description: description,
  };

  // Get a key for a new Post.
  //var newPostKey = firebase.database().ref().child('boxes').push().key;

  // Write the new post's data simultaneously in the posts list and the user's post list.
  // var updates = {};
  // updates['/boxes/' + boxId] = postData;
  //updates['/user-posts/' + uid + '/' + newPostKey] = postData;

  // var result= firebase.database().ref().update(updates);
},
  saveBox:function(){
    var id=guid();
      firebase.database().ref('boxes/'+id).set({
        name:this.name,
        users:[{'name':this.name,'description':this.description,'email':this.email}],
        boxName:this.boxName,
        description:this.description,
        id:id
      })
  },
  hide :function() {
  }
  }
}
function guid() {
  function s4() {
    return Math.floor((1 + Math.random()) * 0x10000)
      .toString(16)
      .substring(1);
  }
  return s4() + s4() + '-' + s4() + '-' + s4() + '-' +
    s4() + '-' + s4() + s4() + s4();
}
</script>
 
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
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
