<template>
  <div id="staffpage">
    <div id="staff_title">Meet the officers!</div>
    <div id="staff_parent">
      <OfficerCard
        v-for="(staff, index) in officer_list"
        :key="index"
        :name="staff.name"
        :callsign="staff.callsign"
        :position="staff.position"
        :info="staff.info"
      />
    </div>
  </div>
</template>

<script>
import OfficerCard from "../components/OfficersCard.vue";
import axios from "@/utils/axiosClient";

export default {
  props: {
    parallax: {
      type: Boolean,
      default: true,
    },
  },
  name: "OfficerPage",
  components: {
    OfficerCard,
  },
  data() {
    return {
      officer_list: [],
      nodata: true,
    };
  },
  methods: {
    scrollToTop() {
      document.body.scrollTop = 0;
    },
    getOfficers(VueOBJ) {
      //call API to load officers from database
      const URL = `https://us-east-1.aws.data.mongodb-api.com/app/app-0-yyrfg/endpoint/officer`;
      axios
        .get(URL)
        .then(function (response) {
          // handle success
          if (!response.data) {
            VueOBJ.nodata = true;
            return;
          }
          console.log(response.data);
          VueOBJ.officer_list = response.data;
        })
        .catch(function (error) {
          // handle error
          error;
        })
        .finally(function () {
          // always executed
        });
    },
  },
  async mounted() {
    this.scrollToTop();
    await this.getOfficers(this);
  },
};
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#staff_title {
  font-size: 4em;
  font-family: "Montserrat";
  text-align: center;
  display: flex;
  justify-content: center;
  align-items: center;
  padding-top: 100px;
  color: white;
}
#staffpage {
  height: fit-content;
  width: 100%;
  background-color: rgb(17, 17, 17);
  padding-left: 90px;
}
#staff_parent {
  display: flex;
  flex-direction: column;
  /* flex-wrap: wrap; */
  padding-top: 10vh;
  padding-bottom: 20vh;
  height: fit-content;
  justify-content: center;
  align-items: center;
  min-height: 80vh;
}

/* Slightly Resized Screen Styles */
@media screen and (max-width: 1200px) {
  #staffpage {
    padding-left: 50px;
  }
  #staff_title {
    font-size: 3em;
  }
}

/* Half-Screen Styles */
@media screen and (max-width: 900px) {
  #staffpage {
    padding-left: 0px;
  }
  #staff_title {
    font-size: 2em;
  }
}

/* Mobile Styles */
@media screen and (max-width: 768px) {
  #staff_title {
    font-size: 1.5em;
  }
}
</style>
