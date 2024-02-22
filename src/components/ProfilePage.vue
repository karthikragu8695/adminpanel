<template>
    <div class="mx-10 my-10">
        <v-btn @click="dialoges">Add Profile</v-btn> 
        <v-card class="mt-10">
        <v-data-table
          :headers="headers"
          :items="items"
          item-value="name" >
            <template v-slot:[`item.dob`]="{ item }">
                {{toLocalDate(item.dob)}}
            </template>
            <template v-slot:[`item.actions`]="{ item }">
              <v-icon
                size="small"
                class="me-2 text-brown"
                @click="editItem(item)">
                mdi-pencil
              </v-icon>
              <v-icon
                size="small"
                class="text-red"
                @click="deleteItem(item)">
                mdi-delete
              </v-icon>
            </template>
        </v-data-table>
        
        </v-card>
            <v-dialog v-model="dialog"  fullscreen :scrim="false" transition="slide-x-reverse-transition">
                <v-list>
                    <v-form ref="form" @submit.prevent="Add">
                 <v-row align="center" justify="center">
                    <v-col justify="center" align="center" cols="12">
                        <v-card >
                            <v-card-text>
                                <v-row class="flex">
                                    <span class="mdi mdi-arrow-left-thick text-h4 ml-5 "  @click="dialog=false"></span><v-spacer/>
                                    <!-- <v-col cols="2" class="bg-red py-3" @click="dialog=false">Cancel</v-col><v-spacer/> -->
                                    <!-- <span class="mdi mdi-window-close text-h4 mr-5"  @click="dialog=false" ></span> -->
                                    <!-- <v-col  cols="2" class="bg-red py-3" @click="dialog=false" >Cancel</v-col> -->
                                </v-row>
                            <v-text-field label="Name" v-model="name" :rules="firstNameRules"  :counter="10" required class="mt-10"  variant="outlined"></v-text-field>
                            <v-radio-group inline v-model="gender"  >
                                    <v-radio label="Male"    class="border border-black pr-5"  value="male"></v-radio>
                                    <v-radio label="Female"  class="border border-black pr-5 ml-5"  value="female"></v-radio>
                            </v-radio-group>
                            <v-row v-model="dob" >
                                <v-col cols="3">
                                <v-select
                                label="Day"
                                :items="['1', '2', '3', '4', '5', '6','7','8','9','10','11','12','13','14','15']"
                                variant="outlined"
                               
                                :rules="dobRules1"
                                required
                                v-model="date">
                            </v-select>
                            </v-col>
                            <v-col cols="4">
                                <v-select
                                label="Month"
                                v-model="Month"
                                :rules="dobRules2"
                                required
                                :items="['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun','Jul','Aug','Sep','Oct','Nov','Dec',]"
                                variant="outlined"
                                ></v-select>
                            </v-col>
                            <v-col cols="5">
                                <v-select
                                label="Year"
                                v-model="year"
                                :rules="dobRules3"
                                required
                                :items="['2002','2001','2000','1999','1998','1997','1996','1995','1994','1993','1992','1991','1990']"
                                variant="outlined"
                                ></v-select>
                            </v-col>
                            </v-row>
                            <v-select  v-model="MaritalStatus" label="Marital Status"
                                    :rules="[v => !!v || 'MaritalStatus is required']"
                                    :items="['Never Married', 'Windower', 'Divorced', 'Awaiting Divorce']"
                                    variant="outlined">
                            </v-select>
                            <v-select
                                v-model="caste"
                                label="Caste"
                                :rules="[v => !!v || 'Caste is required']"
                                :items="['Intercaste', 'Irani', 'Others', 'Parsi','Dont wish to specify']"
                                variant="outlined">
                            </v-select>
                            <v-row>
                                <v-col cols="6">
                                <v-text-field v-model="FatherName" label="Father Name"  :rules="fateherNameRules" required  variant="outlined"></v-text-field>
                                </v-col>
                                <v-col cols="6">
                                <v-text-field v-model="FatherOccupation " label="Father Occupation" :rules="fateherNameOccupation" required variant="outlined"></v-text-field>
                                </v-col>
                            </v-row>
                            
                            <v-row>
                                <v-col cols="6">
                                <v-text-field v-model="MotherName" label="Mother Name"  :rules="motherNameRules" required variant="outlined"></v-text-field>
                                </v-col>
                                <v-col cols="6">
                                <v-text-field v-model="MotherOccupation" label="MotherOccupation"  required :rules="motherOccupation"  variant="outlined"></v-text-field>
                                </v-col>
                            </v-row>
                            <v-row>
                                <v-col cols="6">
                                <v-text-field v-model="age" label="Age" :rules="AgeRules" required variant="outlined"></v-text-field>
                                </v-col>
                                <v-col cols="6">
                                <v-text-field v-model="height" label="Height" :rules="HeightRules" required variant="outlined"></v-text-field>
                                </v-col>
                            </v-row>
                
                            <v-select
                                    label="Religion"
                                    v-model="Religion"
                                    :rules="[v => !!v || 'Religion is required']"
                                    :items="['Hindu', 'Muslim', 'Christian']"
                                    variant="outlined">
                            </v-select>
                            <v-select
                                    label="Education"
                                    v-model="Education"
                                    :rules="[v => !!v || 'Education is required']"
                                    :items="['UG', 'PG', 'SSLC','HSLC','Diplomo',]"
                                    variant="outlined">
                                </v-select>
                                <v-select
                                    label="Profession"
                                    v-model="Profession"
                                    :rules="[v => !!v || 'Profession is required']"
                                    :items="['Doctor', 'IT', 'Non IT','Business','Civil','Goverment Job','Others']"
                                    variant="outlined">
                                </v-select>
                                <v-row>
                                <v-col cols="6">
                                    <v-text-field label="ElderBrother" type="number" v-model="ElderBrothers"  variant="outlined"></v-text-field>
                                </v-col>
                                <v-col cols="6">
                                <v-text-field label="YoungerBrother" v-model="YoungerBrother" type="number" variant="outlined"></v-text-field>
                                </v-col>
                                </v-row>
                                <v-row>
                                <v-col cols="6">
                                    <v-text-field label="ElderSister" type="number" v-model="ElderSister" variant="outlined"></v-text-field>
                                </v-col>
                                <v-col cols="6">
                                <v-text-field label="YoungerSister" v-model="YoungerSister" type="number" variant="outlined"></v-text-field>
                                </v-col>
                                </v-row>
                                
                            <v-text-field label="lives" v-model="lives" :rules="livesRules" required variant="outlined"></v-text-field>
                            <v-text-field label="Rashi" v-model="Rashi" :rules="RashiRules" required variant="outlined"></v-text-field>
                            <v-text-field label="Nadchathiram" v-model="Nadchathiram"  :rules="ThosamRules"  required variant="outlined"></v-text-field>
                            <v-text-field label="Thosam" v-model="Thosam" required variant="outlined"></v-text-field>
                            <v-text-field  v-model="FamilyGod"  label="Family God" variant="outlined"></v-text-field>
                            <v-text-field label="Mobile Number" v-model="mobile" :rules="MobileRules" required  variant="outlined"></v-text-field>
                            </v-card-text>
                            <v-row>
                              <v-col cols="6" >
                                <v-file-input  v-model="photos" prepend-icon="mdi-camera"  variant="outlined" label="Photo Upload"></v-file-input>
                              </v-col>
                            </v-row>
                            <v-card-actions  v-if="!update">
                                <v-btn type="submit" :loading="loading" block variant="outlined" color="green">Add profile</v-btn>
                            </v-card-actions>
                            <v-card-actions v-else>
                              <v-btn @click="updateItems()" :loading="loading" block variant="outlined" color="blue">update</v-btn>
                            </v-card-actions>
                        </v-card>
                    </v-col>
                 </v-row>
            </v-form>
                </v-list>
            </v-dialog>
            <!-- <v-dialog v-model="dialogDelete" max-width="500px">
          <v-card>
            <v-card-title class="text-h5">Are you sure you want to delete this item?</v-card-title>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue-darken-1" variant="text" >Cancel</v-btn>
              <v-btn color="blue-darken-1" variant="text" @click="deleteItemConfirm()">OK</v-btn>
              <v-spacer></v-spacer>
            </v-card-actions>
          </v-card>
        </v-dialog> -->

            
    </div>
</template>
  <script setup>
  import { ref,onMounted } from 'vue'
  import { supabase } from '../supabase'
  const dialog =ref(false)
  // const dialogDelete =ref(false)
  const update = ref(false)
  const loading = ref(false)
  const name = ref('')
  const age = ref('')
  const height = ref('')
  const lives = ref('')
  const mobile = ref('')
  const MaritalStatus = ref(null)
  const caste = ref(null)
  const Religion = ref(null)
  const Education = ref(null)
  const Profession = ref(null)
  const Rashi = ref(null)
  const Nadchathiram = ref(null)
  const Thosam = ref(null)
  const ElderBrothers = ref(null)
  const YoungerBrother = ref(null)
  const ElderSister = ref(null)
  const YoungerSister = ref(null)
  const FatherName = ref(null)
  const MotherName = ref(null)
  const FatherOccupation = ref(null)
  const MotherOccupation = ref(null)
  const FamilyGod = ref(null)
  const gender = ref(null)
  const photos = ref(null)
  const dob =ref(null)
  const date = ref(null)
  const Month = ref(null)
  const year = ref(null)
  const id = ref (null)

  function dialoges(){
    dialog.value=true
    update.value=false
    name.value= null
    age.value = null
    height.value= null
    lives.value = null
    mobile.value = null
    MaritalStatus.value = null
    caste.value = null
    Religion.value = null 
    Education.value = null
    Profession.value = null
    Rashi.value = null
    Nadchathiram.value = null
    Thosam.value = null
    ElderBrothers.value = null
    YoungerBrother.value = null
    ElderSister.value = null
    YoungerSister.value = null
    FatherName.value = null
    MotherName.value = null
    FatherOccupation.value = null
    MotherOccupation.value = null
    FamilyGod.value = null
    gender.value = null
    photos.value = null
    dob.value = null
    date.value = null
    Month.value= null
    year.value = null
  }
   async function updateItems(){
    dialog.value=false
    
    const { data } = await supabase
      .from('profiles')
      .update(
        { name : name.value,
          age: age.value ,
          height:height.value,
          lives:lives.value,
          mobile:mobile.value ,
          MaritalStatus:MaritalStatus.value ,
          caste:caste.value,
          Religion:Religion.value,
          Education:Education.value,
          Profession:Profession.value,
          Rashi:Rashi.value,
          Nadchathiram: Nadchathiram.value,
          Thosam:Thosam.value,
          ElderBrothers:ElderBrothers.value,
          YoungerBrother: YoungerBrother.value,
          ElderSister : ElderSister.value,
          YoungerSister : YoungerSister.value,
          FatherName : FatherName.value,
          MotherName : MotherName.value,
          FatherOccupation:FatherOccupation.value,
          MotherOccupation:MotherOccupation.value,
          FamilyGod:FamilyGod.value ,
          gender:gender.value,
          
         })
      .eq('id', id.value)
      .select()
      console.log(data);
    }
    async function deleteItem (item) {
    // dialogDelete.value = true
    console.log(item);
    
    const { error } = await supabase
      .from('profiles')
      .delete()
      .select('*')
      .eq('id', item.id)
      id.value = item.id
      console.log(error)
          

  }
  // async function deleteItemConfirm(){
  // const { error } = await supabase
  //   .from('profiles')
  //   .delete()
  //   .eq('id')
  //   console.log(error);
    
  // }
  async function editItem (item) {
    update.value = true
    const { data } = await supabase
    .from('profiles')
    .select('*')
    .eq('id', item.id)
    console.log(data)  
    id.value = item.id
    date.value = data[0].date
    Month.value = data[0].Month
    year.value = data[0].year
    name.value = data[0].name
    gender.value = data[0].gender
    age.value = data[0].age
    height.value = data[0].height
    lives.value = data[0].lives
    mobile.value = data[0].mobile
    MaritalStatus.value = data[0].MaritalStatus
    caste.value = data[0].caste
    Religion.value = data[0].Religion
    Education.value = data[0].Education
    Profession.value = data[0].Profession
    Nadchathiram.value = data[0].Nadchathiram
    Thosam.value = data[0].Thosam
    ElderBrothers.value = data[0].ElderBrothers
    YoungerBrother.value = data[0].YoungerBrother
    ElderSister.value = data[0].ElderSister
    YoungerSister.value = data[0].YoungerSister
    FatherName.value = data[0].FatherName
    MotherName.value = data[0].MotherName
    FatherOccupation.value = data[0].FatherOccupation
    MotherOccupation.value = data[0].MotherOccupation
    FamilyGod.value = data[0].FamilyGod
    Rashi.value = data[0].Rashi
    dob.value = data[0].dob
    dialog.value = true
        
    }
  const firstNameRules = [
      value => {
        if (value?.length > 3) return true
        return 'First name must be at least 3 characters.'
      },
    ]
  const fateherNameRules = [
      value => {
        if (value?.length > 3) return true
        return 'Father name must be at least 3 characters.'
      },
    ]
  const motherNameRules = [
      value => {
        if (value?.length > 3) return true
        return 'Mother name must be at least 3 characters.'
      },
    ]
  const motherOccupation = [
      value => {
        if (value?.length > 0) return true
        return 'Mother Occupation not set.'
      },
    ]
  const fateherNameOccupation = [
      value => {
        if (value?.length > 0) return true
        return 'Father Occupation not set.'
      },
    ]
    const dobRules1 = [
      value => {
        if (value) return true
        return 'Date or not set '
      },
    ]
    const dobRules2 = [
      value => {
        if (value) return true
        return 'Month or not set '
      },
    ]
    const dobRules3 = [
      value => {
        if (value) return true
        return 'Year or not set '
      },
    ]
    const AgeRules = [
      value => {
        if (value) return true
        return 'Age or not set '
      },
    ]
    const HeightRules = [
      value => {
        if (value) return true
        return 'height or not set '
      },
    ]
    const livesRules = [
      value => {
        if (value?.length > 4) return true
        return 'lives area not '
      },
    ]
    const RashiRules = [
      value => {
        if (value?.length > 4) return true
        return 'Rashi are not  support'
      },
    ]
    
    const ThosamRules = [
      value => {
        if (value?.length > 4) return true
        return 'Thosam are  not support '
      },
    ]
    const MobileRules = [
      value => {
        if (value?.length > 10) return true
        return 'Mobile number error '
      },
    ]
  const headers = [
      {
        title: 'Id',
        align: 'start',
        key: 'id',
      },
      {
        title: 'Name',
        align: 'end',
        key: 'name',
      },
      {
        title: 'Gender',
        align: 'end',
        key: 'gender',
      },
      {
        title: 'Date of birth',
        align: 'end',
        key: 'dob',
      },
      {
        title: 'MaritalStatus',
        align: 'end',
        key: 'MaritalStatus',
      },
      {
        title: 'Actions',
        key: 'actions',
        sortable: false 
      },]
  const items = ref([])
  const toLocalDate = (date) => {
      return new Date(+date).toLocaleDateString('en-IN')
  }
  console.log(dob)
  onMounted(async() => {
    let { data: profiles, error } = await supabase
    .from('profiles')
    .select('*')
    items.value = profiles
    console.log(profiles);
    console.log(error);
  })
  const Add = async () => {
    try { 
      loading.value = true
      
      // const { data,error } = await supabase.auth.signUp({ 
      //   name: email.value,
      //   password: password.value
      //  })
    
        let profile = {  
              name: name.value, 
              age: age.value, 
              height: height.value, 
              lives: lives.value, 
              photos:"https://myizzcmzjfnzaldgrqgw.supabase.co/storage/v1/object/public/images/photos/"+photos.value[0].name,
              mobile: +mobile.value,
              gender : gender.value,
              MaritalStatus : MaritalStatus.value,
              caste :caste.value,
              Religion :Religion.value,
              Education :Education.value,
              Profession :Profession.value,
              Rashi :Rashi.value,
              Nadchathiram :Nadchathiram.value,
              Thosam :Thosam.value,
              ElderBrothers :ElderBrothers.value,
              YoungerBrother :YoungerBrother.value,
              ElderSister :ElderSister.value,
              YoungerSister :YoungerSister.value,
              FatherName :FatherName.value,
              MotherName :MotherName.value,
              FatherOccupation :FatherOccupation.value,
              MotherOccupation:MotherOccupation.value,
              FamilyGod:FamilyGod.value,
              type:'admin',
              dob: Date.parse(`${date.value} ${Month.value} ${year.value} 00:00:00 GMT`)
        }
        const {data,error}= await supabase.storage.from('images').upload(`photos/${photos.value[0].name}`,photos.value[0])  
        console.log(error)
        console.log(data);
        await supabase
          .from('profiles')
          .insert([profile])
          .select(`name,gender,dob,MaritalStatus`)
        .then((res) => {
          // if(profile){
          //   name.value = profile.name,
          //   gender.value =  profile.gender,
          //   dob.value = profile.dob,
          //   MaritalStatus.value = profile.MaritalStatus
          // }
          // console.log(profile)
          if(!res.error) {
            console.log("login successfull")
          }else{
        console.log('error')
      }
        })
      }
      catch (error) {
      alert(error.error_description || error.message)
    } finally {
      loading.value = false
      dialog.value = false
    }
    } 


  </script>