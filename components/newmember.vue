<template>
  <div>
    <a-form :form="form" @submit="handleSubmit">
      <a-card style="width: auto;height:auto;">
        <a-tabs default-active-key="1">
          <a-tab-pane key="1">
            <span slot="tab">
              <a-icon type="user" />
              Beneficiary information
            </span>
            <a-row>
              <a-col>
                <br />
                <p>
                  Name <br />
                  <a-input
                    placeholder="provide fullnames"
                    v-model="form.name"
                    allow-clear
                    style="margin-top:5px;width:400px"
                  />
                </p>
                <p>
                  Gender <br />

                  <a-select v-model="form.gender" style="width: 400px">
                    <a-select-option value="Male">
                      Male
                    </a-select-option>
                    <a-select-option value="Female">
                      Female
                    </a-select-option>
                  </a-select>
                </p>
                <p>
                  Sub county <br />
                  <a-input
                    placeholder="your sub-county"
                    v-model="form.subcounty"
                    allow-clear
                    style="margin-top:5px;width:400px"
                  />
                </p>
                <p>
                  Ward <br />
                  <a-input
                    placeholder="your ward"
                    v-model="form.ward"
                    allow-clear
                    style="margin-top:5px;width:400px"
                  />
                </p>
                <p>
                  Actual Fees Amount <br />
                  <a-input
                    placeholder="Amount $"
                    v-model="form.actualfees"
                    allow-clear
                    style="margin-top:5px;width:400px"
                  />
                </p>
                <p>
                  Amount Issued <br />
                  <a-input
                    placeholder="Issued $"
                    v-model="form.amountissued"
                    allow-clear
                    style="margin-top:5px;width:400px"
                  />
                </p>
              </a-col>
            </a-row>
          </a-tab-pane>
          <a-tab-pane key="2">
            <span slot="tab">
              <a-icon type="user" />
              School information
            </span>
            <a-row>
              <a-col>
                <br />
                <p>
                  School/College <br />
                  <a-input
                    placeholder="provide school name"
                    v-model="form.school"
                    allow-clear
                    style="margin-top:5px;width:400px"
                  />
                </p>

                <p>
                  Class/Year <br />
                  <a-input
                    placeholder="provide the class"
                    v-model="form.year"
                    allow-clear
                    style="margin-top:5px;width:400px"
                  />
                </p>

                <p>
                  Course <br />
                  <a-input
                    placeholder="provide the course name"
                    v-model="form.course"
                    allow-clear
                    style="margin-top:5px;width:400px"
                  />
                </p>

                <p>
                  Remarks <br />
                  <a-input
                    placeholder="remarks "
                    v-model="form.remarks"
                    allow-clear
                    style="margin-top:5px;width:400px"
                  />
                </p>
              </a-col>
            </a-row>
          </a-tab-pane>
        </a-tabs>
        <a-divider />
        <a-button type="primary" html-type="submit">
          Submit information
        </a-button>
        <br>
        <br>
        <p>
          <a-alert message="" v-if="errors.length" closable type="error">
          <p slot="description">
            <span style="color: red">
             Errors 
               <ul>
        <li v-for="error in errors" v-bind:key="error">{{error}}</li>
      </ul>
            </span>
          </p>
        </a-alert>
        </p>

         
      </a-card>
     
    </a-form>
  </div>
</template>

<script>
import * as firebase from 'firebase/app'
import 'firebase/firestore'

export default {
  data() {
    return {
      errors:[],
      formLayout: "horizontal",
      form: this.$form.createForm(this, { name: "coordinated" }),
      form: {
        name: "",
        gender: "",
        subcounty: "",
        ward: "",
        actualfees: "",
        amountissued: "",
        school: "",
        year: "",
        course: "",
        remarks: ""
      }
    };
  },
  methods: {
    handleSubmit(e)
    {
        e.preventDefault();
        //validate bsic
          this.errors = [];
        //check the common errors if empty
         if(this.form.name==="")
        {
          this.errors.push('Name cannot be empty');
        }
           if(this.form.gender==="")
        {
          this.errors.push('Gender cannot be empty');
        }
           if(this.form.ward==="")
        {
          this.errors.push('Ward cannot be empty');
        }
           if(this.subcounty==="")
        {
          this.errors.push('Subcounty cannot be empty');
        }
        if(this.form.actualfees==="")
        {
          this.errors.push('actualfees cannot be empty');
        }
         if(this.form.amountissued==="")
        {
          this.errors.push('amountissued cannot be empty');
        }
         if(this.form.school==="")
        {
          this.errors.push('school cannot be empty');
        }
         if(this.form.course==="")
        {
          this.errors.push('course cannot be empty');
        }
          if(this.form.year==="")
        {
          this.errors.push('year cannot be empty');
        }
        
           if(this.errors.length>0)
        {
           return;
        }

        var StoreDB = firebase.firestore();
      StoreDB.collection("beneficiaries")
        .add({
          Name: this.form.name,
          Gender: this.form.gender,
          Ward: this.form.ward,
          Subcounty:this.form.subcounty,
          ActualFeesAmount:this.form.actualfees,
          AmountIssued:this.form.amountissued,
          School:this.form.school,
          Course:this.form.course,
          Year:this.form.year,
          Remarks:this.form.remarks
        })
        .then(function(docRef) 
        {
          
          console.log("Document written with ID: ", docRef.id);
        })
        .catch(function(error) 
        {
          console.error("Error adding document: ", error);
        });
        this.$message.info("Beneficiary information successfuly added.");
    }
  }
};
</script>

<style scoped></style>
