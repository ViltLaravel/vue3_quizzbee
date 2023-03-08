<template>
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-EVSTQN3/azprG1Anm3QDgpJLIm9Nao0Yz1ztcQTwFspd3yD65VohhpuuCOmLASjC" crossorigin="anonymous">
    <div class="header">
      <div>
        <img src="http://1.bp.blogspot.com/-gFDOlz9yCt0/T21mywVpHII/AAAAAAAADZU/b7SQsH7c94o/s1600/BISU-Logo.png" alt="" style="width:100px">
      </div>
      <div>
        <span style="color: #fff;">Leaderboards</span>
      </div>
      <div>
        <img src="https://bisu7.bisubilar.org/wp-content/uploads/2021/08/CTAS-logo-copy-copy-1024x1024.jpg" alt="" style="width: 100px ; border-radius: 50%;">
      </div>
    </div>
    <div>
      <div class="box" style="margin-top: 1rem; border-radius: 20px;">

        <!--START BOX HEADER -->
        <div class="head" style="font-size: 2.5em; color: #FFCC00; font-weight: bold;">
          <span style="width: 80%;">Name</span>
          <span style="width: 20%; padding-left: 2rem;">Points</span>
        </div>
        <!-- END BOX HEADER -->

        <div style="margin-bottom: 0px;" v-for="participant in participants" :key="participant.id" class="score">
          <!-- START OF NAME PARTICIPANTS -->
            <div class="name">
              <span>{{ participant.name }}</span>
                <div class="progress" style="width: 100%;">
                  <div class="progress-bar bg-warning" role="progressbar" v-bind:style="{width: participant.score + '%'}" aria-valuenow="10" aria-valuemin="100" aria-valuemax="100"></div>
                </div>
            </div>
          <!-- END OF NAME PARTICIPANTS -->

          <!-- START OF PARTICIPANTS POINTS -->
            <div class="point">
              <span class="postName" style="color: #FFCC00; visibility: hidden;">&nbsp;</span>
              <span>{{ participant.score }}</span>
            </div>
          <!-- END OF POINTS -->
        </div>

      </div>
    </div>
  </template>
  
  <script>
  import axios from 'axios';

export default {
  name: 'LandingPage',
  data() {
    return {
      participants: []
    }
  },
  
  mounted() {
    this.fetchData(); // Call fetchData() once on mount

    // Call fetchData() every second
    setInterval(() => {
      this.fetchData();
    }, 1000);

    this.$nextTick(() => {
      const posNameElements = this.$el.querySelectorAll('.posName');
      posNameElements.forEach((element) => {
        const txt = element.textContent.trim();
        if (this.seen[txt]) {
          element.parentNode.removeChild(element);
        } else {
          this.seen[txt] = true;
        }
      });
    });
  },
  methods: {
    fetchData() {
      axios.get('https://quizzbee.bisubilar.org/api/participant')
        .then(response => {
          this.participants = response.data;
        })
        .catch(error => {
          console.error('Error retrieving participants:', error);
        });
    }
  }
}


  </script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped>
     @import url('https://fonts.googleapis.com/css2?family=Jost:wght@300;500&display=swap');

     
     .header{
      display: flex;
      justify-content: space-evenly;
      align-items: center;
      font-size: 2em;
      text-transform: uppercase;
      font-weight: bold;
      padding-top: 1rem;
     }
     .head{
      margin: auto;
      color: hsl(0, 0%, 100%);
      width: 50%;
      display: flex;
      align-items: center;
      justify-content: space-between;
      /* border: 2px solid white; */
     }
     .box{
      display: flex;
      justify-content: space-evenly;
      align-items: center;
      flex-direction: column;
      gap: 1rem;
      background-color: #330066;
      padding-bottom: 4rem;
      padding-top: 4rem;
      width: 90%;
      margin: auto;
      border: 2px solid #fff;
      

     }
     .score{
      display: flex;
      justify-content: center;
      align-items: center;
      width: 50%;
      color: #fff;
      font-size: 2em;
      
     
      
     }
     .name{
      display: flex;
      justify-content: center;
      align-items: left;
      flex-direction: column;
      width: 80%;
      
     }
     .point{
      display: flex;
      justify-content: center;
      align-items: center;
      flex-direction: column;
      width: 20%;
      font-size: 2.5em;
     }
     
  </style>
  