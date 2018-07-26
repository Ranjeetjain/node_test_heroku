<template>

  <div id="createEvents">
  <nav class="navbar navbar-expand-lg navbar-dark navbar-custom">
    <div class="container">
      <a class="navbar-brand" href="#">AIbizConnect</a>
      <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarResponsive" aria-controls="navbarResponsive" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarResponsive">
        <ul class="navbar-nav ml-auto">
          <li class="nav-item">
            <a class="nav-link"  data-toggle="modal" data-target="#modalRegisterForm" href="#">Create Event</a>
          </li>
          <li class="nav-item">
            <a class="nav-link" href="#">Participate Event</a>
          </li>
        </ul>
      </div>
    </div>
  </nav>

  <div class="modal fade" id="modalRegisterForm">
    <div class="modal-dialog modal-dialog-centered">
      <div class="modal-content">
        <div class="modal-header text-center">
          <h4 class="modal-title w-100 font-weight-bold">Create Event</h4>
          <button type="button" class="close" data-dismiss="modal" aria-label="Close">
            <span aria-hidden="true">&times;</span>
          </button>
        </div>

        <div class="modal-body">
          <form class="">
            <div class="row">
              <div class="col-md-6">
                <input type="text" class="form-control" v-model="event.eventName" placeholder="Event name" required>
              </div>
              <div class="col-md-6">
                <input type="text" class="form-control" v-model="event.productName" placeholder="Product name" required>
              </div>
            </div>


            <div class="row">
              <div class="col-md-6">
                <input type="text" class="form-control" v-model="event.companyName" placeholder="Company name" required>
              </div>
              <div class="col-md-6">
                <input type="text" class="form-control" v-model="event.companyURL" placeholder="Company URL" required>
              </div>
            </div>
            <input type="text" class="form-control" v-model="event.companyAddress" placeholder="Company Address" required>
            <input type="date" class="form-control" v-model="event.eventDate" placeholder="Event Date" required>

            <div class="row">
              <div class="col-md-6">
                <input type="text" class="form-control" v-model="event.speakerName" placeholder="Speaker name" required>
              </div>
              <div class="col-md-6">
                <input type="text" class="form-control" v-model="event.speakerLinkedinURL" placeholder="Speaker LinkedIn URL" required>
              </div>
            </div>
            <input type="text" class="form-control" v-model="event.eventDescription" placeholder="Event Description" required>
            <button class="btn btn-success " type="submit" data-dismiss="modal" @click="addEvent">Create</button>

          </form>
        </div>
      </div>
    </div>


  </div>
  <div>
    <div>
      <div class="list-group">
          <div class="row event_row"v-for="(event,index) in events">
            <div class="col-md-6">
          <h6>Event : {{event.eventName}}</h6><hr>
          <h6>Product : {{event.productName}}</h6><hr>
          <h6>Description : {{event.eventDescription}}</h6><hr>
          <h6>Company Name : {{event.companyName}}</h6><hr>
            </div>
            <div class="col-md-6">
          <h6>Date : {{event.eventDate}}</h6><hr>
          <h6>Company Address : {{event.companyAddress}}</h6><hr>
          <h6>Speaker Name : {{event.speakerName}}</h6><hr>
          <h6>LinkedIn URL : {{event.speakerLinkedinURL}}</h6><hr>
            </div>
        </div>
      </div>
      <hr>
    </div>
  </div>
  </div>
</template>
<style media="screen">
.navbar-custom {
  padding-top: 1rem;
  padding-bottom: 1rem;
  background-color: rgba(0,0,0,.7);
}
.form-control{
  margin-bottom: 15px;
}
.event_row{
  margin-top: 30px;
  margin-bottom: 30px;
}
</style>
<script type="text/javascript">
var _axios2 = require('axios');
var GET_DATA_URL = "http://localhost:5000/getData";
var ADD_DATA_URL = "http://localhost:5000/addData";

exports.default = {
  name: 'app',
  data: function data() {
    return {
      event: { eventName: '', productName: '', companyName: '', companyURL: '',
        companyAddress: '', eventDate: '', speakerName: '', speakerLinkedinURL: '', eventDescription: '' },
      events: []
    };
  },
  created: function created() {
    var _this = this;
    _axios2.default.get(GET_DATA_URL).then(function (response) {
      var result = response.data;
      for (var i = 0; i < result.length; i++) {
        _this.event = { eventName: result[i].eventName, productName: result[i].productName,companyName: result[i].companyName, companyURL: result[i].companyURL,
          companyAddress: result[i].companyAddress, eventDate: result[i].eventDate, speakerName: result[i].speakerName, speakerLinkedinURL: result[i].speakerLinkedinURL,eventDescription: result[i].eventDescription };
        _this.events.push(_this.event);
      }
      _this.event = { eventName: '', productName: '', companyName: '', companyURL: '', companyAddress: '', eventDate: '', speakerName: '', speakerLinkedinURL: '', eventDescription: '' };
    }, function (error) {
      console.log("error");
    });
  },

  methods: {
    addEvent: function addEvent() {
      var _this2 = this;
      this.events.push(this.event);
      var newData = JSON.stringify(this.event);
      _axios2.default.post(ADD_DATA_URL, JSON.parse(newData)).then(function (response) {
        console.log(response);
        _this2.event = { eventName: '', productName: '', companyName: '', companyURL: '',
          companyAddress: '', eventDate: '', speakerName: '', speakerLinkedinURL: '', eventDescription: '' };
      }, function (error) {
        console.log("Error");
      });
    }
  },
};
</script>
