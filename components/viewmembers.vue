<template>
  <div>
    <a-card style="width: auto;height:auto;">
      <h2>Beneficiaries list</h2>
      <a-divider />
      <a-table :columns="columns" :data-source="datatables" bordered small>
        <span slot="action" slot-scope="text, record">
          <a-button type="danger" @click="deletemember(record)">
            Delete
          </a-button>
        </span>
      </a-table>
    </a-card>
  </div>
</template>

<script>
import * as firebase from "firebase/app";
import "firebase/firestore";

export default {
  data() {
    return {
      datatables: [],
      columns: []
    };
  },
  methods: {
    loadmembers() {
      var list = [];
      //clear columns first
      this.columns = [];
      //load column names
      this.columns = [
        {
          title: "Name",
          dataIndex: "Name",
          key: "Name"
        },
        {
          title: "Gender",
          dataIndex: "Gender",
          key: "Gender"
        },
        {
          title: "Ward",
          dataIndex: "Ward",
          key: "Ward"
        },
        {
          title: "Subcounty",
          key: "Subcounty",
          dataIndex: "Subcounty"
        },
        {
          title: "School",
          key: "School",
          dataIndex: "School"
        },
        {
          title: "Course",
          key: "Course",
          dataIndex: "Course"
        },
        {
          title: "FeesAmount",
          key: "FeesAmount",
          dataIndex: "FeesAmount"
        },
        {
          title: "Issued",
          key: "AmountIssued",
          dataIndex: "AmountIssued"
        },
        {
          title: "Year",
          key: "Year",
          dataIndex: "Year"
        },
        {
          title: "Remarks",
          key: "Remarks",
          dataIndex: "Remarks"
        },

        {
          title: "Action",
          key: "action",
          scopedSlots: { customRender: "action" }
        }
      ];
      this.data = [];
      var db = firebase.firestore();
      var mem = db
        .collection("beneficiaries")
        .get()
        .then(function(querySnapshot) {
          querySnapshot.forEach(function(doc) {
            // doc.data() is never undefined for query doc snapshots
            // console.log(doc.id, " => ", doc.data().Name + ' '+doc.data().Gender+' '+doc.data().Ward);
            var name = doc.data().Name;
            var gender = doc.data().Gender;
            var ward = doc.data().Ward;
            var subcounty = doc.data().Subcounty;
            var school = doc.data().School;

            list.push({
              Id: doc.id,
              Name: name,
              Gender: gender,
              Ward: ward,
              Subcounty: subcounty,
              School: school,
              Course: doc.data().Course,
              FeesAmount: doc.data().ActualFeesAmount,
              AmountIssued: doc.data().AmountIssued,
              Year: doc.data().Year,
              Remarks: doc.data().Remarks
            });
          });
        })
        .catch(function(error) {
          console.log("Error getting documents: ", error);
        });
      this.datatables = list;
    },
    deletemember(row) 
    {
      var db = firebase.firestore();
      console.log(row);
      db.collection("beneficiaries")
        .doc(row.Id)
        .delete()
        .then(function() {
       
          console.log("successfully deleted!");
       
        })
        .catch(function(error) {
          console.error("Error removing document: ", error);
        });
            this.$message.info("successfully deleted the beneficiary");
         // alert("successfully deleted the member");
          this.loadmembers();
    }
  },
  created() {
    this.loadmembers();
  }
};
</script>

<style scoped></style>
