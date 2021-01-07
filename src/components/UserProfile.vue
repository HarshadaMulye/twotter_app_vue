<template>
  <div class="user-profile">
      <div class="user-profile_user-panel">
          <h1 class="user-profile_username"> @{{user.userName}}</h1>
          <div class="user-profile_admin-badge" v-if="user.isAdmin">
              Admin 
          </div>
 <div class="user-profile_follower-count">
   <strong>Followers</strong>{{followers}}
 </div>
 <form class="user-profile_create-twoot" @submit.prevent="createNewTwoot" :class="{'exceeded':newTwootCharacterCount>180}">
   <label for="newTwoot"><strong>New Twoot</strong>({{newTwootCharacterCount}}/180)</label>
   <textarea id="newTwoot" rows="4" v-model="newTwootContent"/>
   <div class="user-profile_create-twoot-type">
     <label for="newTwootType"><strong>Type:</strong></label>
     <select id="newTwootType" v-model="selectedTwootType">
       <option :value="option.value" v-for="(option,index) in twootTypes" :key="index">
         {{option.name}}
       </option>
     </select>
   </div>
   <button>
     Twoot!
   </button>
 </form>
  </div>
 <div class="user-profile_twoots-wrapper">
     <TwootItem v-for="twoot in user.twoots" :key="twoot.id" :username="user.userName" :twoot="twoot" @favourite="toggleFavourite"/>
        </div>
  </div>
</template>

<script>
import TwootItem from "./TwootItem"
export default {
  name: "UserProfile",
  components:{TwootItem},
  data() {
    return {
      newTwootContent:'',
      selectedTwootType:'instant',
      followers: 0,
      twootTypes:[
        {value:"draft" ,name:"Draft"},
        {value:"instant" ,name:"Instant Twoot"}
      ],
      user: {
        id: 1,
        userName: "_HarshadaMulye",
        firstName: "Harshada",
        lastName: "Mulye",
        email: "mulyeharshada@gmail.com",
        isAdmin: true,
        twoots:[
            {id:1,content:"I am best"},
            {id:2,content:"reva"}
        ]
      },
    };
  },
  computed: {
    fullName() {
      return `${this.user.firstName} ${this.user.lastName}`;
    },
    newTwootCharacterCount(){
      return this.newTwootContent.length;
    },
  },
  methods: {
    followUsers() {
      this.followers++;
    },
    toggleFavourite(id)
    {
        console.log("favourite twoot",id)
    },
    createNewTwoot()
    {
      if(this.newTwootContent && this.selectedTwootType!=='Draft')
      {
        this.user.twoots.unshift({
          id:this.user.twoots.length+1,
          content:this.newTwootContent
        })
        this.newTwootContent=''
      }

    }
  },
  watch: {
    followers(newfollowerCount, oldfollowerCount) {
      if (oldfollowerCount < newfollowerCount) {
        console.log(`${this.user.firstName} has gained a follower`);
      }
    },
  },
 
  mounted() {
    this.followUsers();
  },
};
</script>

<style>
.user-profile{
display: grid;
grid-template-columns: 1fr 3fr;
width:100%;
padding: 50px 5%;
}
.user-profile_user-panel{
    display: flex;
    flex-direction: column;
    margin-right: 50px;
    padding:20px;
    background-color: white;
    border-radius: 5px;
    border:1px solid #DFE3EB;
}
h1{
    margin:0;
}
.user-profile_admin-badge{
    background:rebeccapurple;
    color:white;
    border-radius: 5px;
    margin-right: auto;
    padding:0px 10px;
    font-weight: bold;
    
}
.user-profile_create-twoot{
  display: flex;
  flex-direction: column;
  border-top:1px solid #DFE3EB;
  padding-top:20px;
}
.user-profile_twoots-wrapper{
  display: grid;
  grid-gap:10px;

}
.exceeded{
  color:red;
 
}
</style>
