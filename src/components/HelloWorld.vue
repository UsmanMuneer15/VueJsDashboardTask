<template>
  <v-card> <v-layout>
      <v-navigation-drawer expand-on-hover> <v-list> <v-list-item class="back"
            prepend-avatar="https://randomuser.me/api/portraits/women/85.jp" title="Usman Muneer"
            subtitle="usman@gmailcom"></v-list-item>
        </v-list> <v-divider>

        </v-divider> <v-list density="compact" nav> <v-list-item class="back" prepend-icon="fa fa-home" title="Home"
            value="myfiles"></v-list-item>
          <v-list-item  class="back" prepend-icon="fa fa-address-card" title="About" value="shared"></v-list-item>
          <v-list-item class="back" prepend-icon="fa fa-sign-in" title="Login" value="starred">
            
          </v-list-item>
        </v-list>
      
      </v-navigation-drawer>
      <v-main>
      <v-card>
         <!-- .......camera -->
        <div id="app" class="web-camera-container">
  <div class="camera-button">
      <button type="button" class="button is-rounded" :class="{ 'is-primary' : !isCameraOpen, 'is-danger' : isCameraOpen}" @click="toggleCamera">
        <span v-if="!isCameraOpen">Open Camera</span>
        <span v-else>Close Camera</span>
    </button>
  </div>
  
  <div v-show="isCameraOpen && isLoading" class="camera-loading">
    <ul class="loader-circle">
      <li></li>
      <li></li>
      <li></li>
    </ul>
  </div>
  
  <div v-if="isCameraOpen" v-show="!isLoading" class="camera-box" :class="{ 'flash' : isShotPhoto }">
    
    <div class="camera-shutter" :class="{'flash' : isShotPhoto}"></div>
      
    <video v-show="!isPhotoTaken" ref="camera" :width="450" :height="337.5" autoplay></video>
    
    <canvas v-show="isPhotoTaken" id="photoTaken" ref="canvas" :width="450" :height="337.5"></canvas>
  </div>
  
  <div v-if="isCameraOpen && !isLoading" class="camera-shoot">
    <button type="button" class="button" @click="takePhoto">
      <img src="https://img.icons8.com/material-outlined/50/000000/camera--v2.png">
    </button>
  </div>
  
  <div v-if="isPhotoTaken && isCameraOpen" class="camera-download">
    <a id="downloadPhoto" download="my-photo.jpg" class="button" role="button" @click="downloadImage">
      Download
    </a>
  </div>
</div>
        <v-layout>
            <nav class="navbar navbar-expand-lg navbar-light bg-green">
              <a class="navbar-brand" href="#">Logo</a>
              <button class="navbar-toggler" type="button" data-toggle="collapse"
                data-target="#navbarSupportedContent"
                aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation"> <span
                  class="navbar-toggler-icon">
                </span>
              </button>
              <div class="collapse navbar-collapse" id="navbarSupportedContent">
                <ul class="navbar-nav mr-auto">
                  <!-- <li class="nav-item active">        <a class="nav-link" href="#">Home <span class="sr-only">(current)</span></a>      </li>      <li class="nav-item">        <a class="nav-link" href="#">About</a>      </li> -->
                </ul>
                <form class="form-inline my-2 my-lg-0"> <input class="form-control mr-sm-2" type="search"
                    placeholder="Search" aria-label="Search"> <button class="btn btn-outline-success my-2 my-sm-0"
                    type="submit">Search</button> </form>
              </div>
            </nav>
          </v-layout>
          <div class="container-fluid">
            <div class="row">
              <div class="col-lg-4">
                <div class="classone">
                  <h4>Info</h4>
                  <p>Lorem, ipsum dolor sit amet consectetur adipisicing elit. Illum minus debitis laboriosam iure id
                    eaque quibusdam obcaecati ipsum veniam, ut delectus, consequuntur in esse rem, eum eius commodi
                    soluta. Sequi eum harum magni, possimus dolorem eligendi impedit, architecto nostrum accusantium amet
                  .</p>
                </div>
              </div>
              <div class="col-lg-4">
                <div class="classone">
                  <h4>Contact</h4>
                  <p>Lorem, ipsum dolor sit amet consectetur adipisicing elit. Illum minus debitis laboriosam iure id
                    eaque quibusdam obcaecati ipsum veniam, ut delectus, consequuntur in esse rem, eum eius commodi
                    soluta. Sequi eum harum magni, possimus dolorem eligendi impedit, architecto nostrum accusantium amet
                    .</p>
                </div>
              </div>
              <div class="col-lg-4">
                <div class="classone">
                  <h4>Text Area</h4>
                  
                  <textarea name="" id="" cols="30" rows="10" v-model="value" style="background-color: gold;">

                  </textarea>
                 <v-btn v-on:click=" getData ()">sumbit</v-btn>
                  
                </div>
              </div>
            </div>
          </div>
        </v-card> </v-main> </v-layout> </v-card>
</template>
<script>
const STORAGE_KEY = 'vue-todo-app-storage';
export default {
   name: 'HelloWorld',
   data()
   {
    return {
      value:null
    }
   
   },
   created (){
      this.value=JSON.parse(localStorage.getItem(STORAGE_KEY) || '[]');
    },
     methods:{
      getData() {
        console.log("the values i:",this.value);
        // this.value.push(value);
        // localStorage.setItem('value',JSON.stringify(this.value));
      }
      
      
     },
     data() {
    return {
      isCameraOpen: false,
      isPhotoTaken: false,
      isShotPhoto: false,
      isLoading: false,
      link: '#'
    }
  },
  
  methods: {
    toggleCamera() {
      if(this.isCameraOpen) {
        this.isCameraOpen = false;
        this.isPhotoTaken = false;
        this.isShotPhoto = false;
        this.stopCameraStream();
      } else {
        this.isCameraOpen = true;
        this.createCameraElement();
      }
    },
    
    createCameraElement() {
      this.isLoading = true;
      
      const constraints = (window.constraints = {
				audio: false,
				video: true
			});


			navigator.mediaDevices
				.getUserMedia(constraints)
				.then(stream => {
          this.isLoading = false;
					this.$refs.camera.srcObject = stream;
				})
				.catch(error => {
          this.isLoading = false;
					alert("May the browser didn't support or there is some errors.");
				});
    },
    
    stopCameraStream() {
      let tracks = this.$refs.camera.srcObject.getTracks();

			tracks.forEach(track => {
				track.stop();
			});
    },
    
    takePhoto() {
      if(!this.isPhotoTaken) {
        this.isShotPhoto = true;

        const FLASH_TIMEOUT = 50;

        setTimeout(() => {
          this.isShotPhoto = false;
        }, FLASH_TIMEOUT);
      }
      
      this.isPhotoTaken = !this.isPhotoTaken;
      
      const context = this.$refs.canvas.getContext('2d');
      context.drawImage(this.$refs.camera, 0, 0, 450, 337.5);
    },
    
    downloadImage() {
      const download = document.getElementById("downloadPhoto");
      const canvas = document.getElementById("photoTaken").toDataURL("image/jpeg")
    .replace("image/jpeg", "image/octet-stream");
      download.setAttribute("href", canvas);
    },
  }
     
     }
     

  
</script>
<style scoped>
.back {
  background-color: rgb(209, 216, 223);
}

.Mainone {
  font-size: large;
  margin-left: 100px;
}



.form-inline {
  float: inline-end;
}

.navbar {
  width: 100%;
}

.classone {
  border: 1px solid blue;
  margin-top: 20px;
  padding: 15px;
  border-radius: 12px;
  width: 100%;
  height: 400px;
  background-color: bisque;
}

.classone h4 {
  text-align: center;
}
</style>