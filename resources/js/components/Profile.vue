<style>
.widget-user-header{
background: url('/img/bg.jpg') center center;
background-size: cover;
height: 250px !important;
}
</style>


<template>
    <div class="container">
        <div class="row">
            <div class="col-md-12 mt-5">
                <!-- card profile -->
                    <div class="card card-widget widget-user">
                        <!-- Add the bg color to the header using any of the bg-* classes -->
                        <div class="widget-user-header text-white">
                            <h3 class="widget-user-username">{{this.form.name}}</h3>
                            <h5 class="widget-user-desc">{{this.form.type}}</h5>
                        </div>
                        <div class="widget-user-image">
                            <img class="img-circle" :src="getProfilePhoto()" alt="User Avatar">
                        </div>
                        <div class="card-footer">
                            <div class="row">
                            <div class="col-sm-4 border-right">
                                <div class="description-block">
                                <h5 class="description-header">3,200</h5>
                                <span class="description-text">SALES</span>
                                </div>
                                <!-- /.description-block -->
                            </div>
                            <!-- /.col -->
                            <div class="col-sm-4 border-right">
                                <div class="description-block">
                                <h5 class="description-header">13,000</h5>
                                <span class="description-text">FOLLOWERS</span>
                                </div>
                                <!-- /.description-block -->
                            </div>
                            <!-- /.col -->
                            <div class="col-sm-4">
                                <div class="description-block">
                                <h5 class="description-header">35</h5>
                                <span class="description-text">PRODUCTS</span>
                                </div>
                                <!-- /.description-block -->
                            </div>
                            <!-- /.col -->
                            </div>
                            <!-- /.row -->
                        </div>
                    </div>
                <!-- end card profile -->
            </div>
            <!-- end col -->
        <div class="col-12">
            <div class="card">
              <div class="card-header p-2">
                <ul class="nav nav-pills">
                  <li class="nav-item"><a class="nav-link active show" href="#activity" data-toggle="tab">Activity</a></li>
                  <li class="nav-item"><a class="nav-link" href="#settings" data-toggle="tab">Settings</a></li>
                </ul>
              </div><!-- /.card-header -->
              <div class="card-body">
                <div class="tab-content">
                  <div class="tab-pane active show" id="activity">

                      <h4> Display Activity </h4>

                  </div>
                  <!-- /.tab-pane -->
                 
                  <div class="tab-pane" id="settings">

                    <form class="form-horizontal">

                      <div class="form-group">
                        <label for="inputName" class="col-sm-2 control-label">Name</label>

                        <div class="col-sm-10">
                          <input v-model="form.name" type="text" class="form-control" id="inputName" placeholder="Name">
                        </div>
                      </div>

                      <div class="form-group">
                        <label for="inputEmail" class="col-sm-2 control-label">Email</label>

                        <div class="col-sm-10">
                          <input v-model="form.email" type="email" class="form-control" :class="{ 'is-invalid': form.errors.has('email') }"
                           id="inputEmail" placeholder="Email">
                          <has-error :form="form" field="email"></has-error>
                        </div>
                      </div>
                      
                      <div class="form-group">
                        <label for="inputExperience" class="col-sm-2 control-label">Experience</label>

                        <div class="col-sm-10">
                          <textarea v-model="form.bio" class="form-control" id="inputExperience" placeholder="Experience"></textarea>
                        </div>
                      </div>

                      <div class="form-group">
                        <label for="inputPS" class="col-sm-2 control-label">Profile Picture</label>

                        <div class="col-sm-10">
                          <input type="file" @change="profilePic" class="form-control" id="inputPS" placeholder="Profile Picture">
                        </div>
                      </div>
                    
                     <div class="form-group">
                        <label for="inputPass" class="col-sm-2 control-label">Password</label>

                        <div class="col-sm-10">
                          <input type="password" v-model="form.password" name="password" class="form-control" :class="{ 'is-invalid': form.errors.has('password') }" id="inputPass" placeholder="Password">
                          <has-error :form="form" field="password"></has-error>
                        </div>
                      </div>

                      <div class="form-group">
                        <div class="col-sm-offset-2 col-sm-10">
                          <button @click.prevent="updateInfo" type="submit" class="btn btn-success">Update</button>
                        </div>
                      </div>
                    </form>
                  </div>
                  <!-- /.tab-pane -->
                </div>
                <!-- /.tab-content -->
              </div><!-- /.card-body -->
            </div>

            <!-- end col -->
        </div>
        <!-- end row -->
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                form: new Form({
                id: '',
                name: '',
                email: '',
                password: '',
                type: '',
                bio: '',
                photo: '',
             })
            }
        },
        methods:{
            getProfilePhoto(){
              let photo = (this.form.photo.length > 200) ? this.form.photo : "img/profile/" + this.form.photo;
              // return "img/profile/" + this.form.photo;
              return photo;
            },
            updateInfo(){
                this.$Progress.start();
                this.form.put('api/profile')
                .then(() => {
                Swal.fire(
                  'Updated!',
                  'Info has been updated.',
                  'success'
                )
                this.$Progress.finish();
                })
                .catch(() => {
                this.$Progress.fail();
                });
            },
            profilePic(e){
                let file = e.target.files[0];
                    console.log(file);
                let reader = new FileReader();
                if(file ['size'] < 2111775){
                    reader.onloadend = (file) => {
                    // console.log('RESULT', reader.result)
                    this.form.photo = reader.result;
                    }
                reader.readAsDataURL(file);
                }else{
                  Swal.fire({
                  type:'error',
                  title: 'Oops...',
                  text:'File must be maximum of 2MB only',
                  })
                }
            }
        }, 
        mounted() {
            console.log('Component mounted.')
        },
        created(){
            axios.get('api/profile')
            .then(({data}) => (this.form.fill(data)) );
        }
    }
</script>
