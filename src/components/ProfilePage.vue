<template> 
<div class="px-10 mt-10">
        <v-btn @click="dialog=true">Add Profile</v-btn> 
        <v-card class="mt-10">
        <v-data-table
          :headers="headers"
          :items="items"
          item-value="name">
            <template v-slot:[`item.dob`]="{ item }">
                {{toLocalDate(item.dob)}}
            </template>
        </v-data-table>
        </v-card>
            <v-dialog v-model="dialog"  fullscreen :scrim="false" transition="slide-x-reverse-transition">
                <!-- <v-list>
                    <v-form ref="form" @submit.prevent="Add">
                        <v-row align="center" justify="center">
                            <v-col justify="center" align="center" cols="12">
                                <v-card >
                                    <v-card-text>
                                        <v-row class="flex">
                                            <v-col cols="2" class="bg-red py-3" @click="dialog=false">Cancel</v-col><v-spacer/>
                                            <v-col  cols="2" class="bg-green py-3" >Save</v-col>
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
                                        :items="['2002', '2001', '2000', '1999', '1998', '1997','1996','1995','1994','1993','1992','1991','1990']"
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
                                    <v-card-actions>
                                        <v-btn type="submit" :loading="loading" block variant="outlined" color="green">Add profile</v-btn>
                                    </v-card-actions>
                                </v-card>
                            </v-col>
                        </v-row>
                    </v-form>
                </v-list> -->
            hiii
            </v-dialog>
    </div>
</template>

<script setup>
import { ref } from 'vue'
const dialog = ref (false)

</script>