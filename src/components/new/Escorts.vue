<template>
  <b-container fluid>
    <!-- NAVBAR-->
<nav class="navbar navbar-expand-lg py-3 navbar-dark bg-dark shadow-sm">
  <div class="container">
    <a href v-on:click="index" class="navbar-brand">
      <!-- Logo Image -->
      <img src="@/assets/logo.svg" width="45" alt="" class="d-inline-block align-middle mr-2">
      <!-- Logo Text -->
      <span class="text-uppercase font-weight-bold">Refundable</span>
    </a>

    <button type="button" data-toggle="collapse" data-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation" class="navbar-toggler"><span class="navbar-toggler-icon"></span></button>

    <div id="navbarSupportedContent" class="collapse navbar-collapse">
      <ul class="navbar-nav ml-auto">
        <li class="nav-item"><a href v-on:click="index" class="nav-link">Home </a></li>
        <li class="nav-item"><a href v-on:click="accountView" class="nav-link">Konto </a></li>
        <li class="nav-item"><a href v-on:click="logout" class="nav-link">Abmelden</a></li>
      </ul>
    </div>
  </div>
</nav>
    <b-row align-h="center">
      <b-col cols="12">
        <b-container fluid>
          <b-row>
            <b-col cols="12">
              <!-- Verzeichnisanzeige -->
              <b-breadcrumb style="background-color: white">
                <b-breadcrumb-item v-on:click="uebersicht"
                  >Antrag Übersicht</b-breadcrumb-item
                >
                <b-breadcrumb-item v-on:click="school"
                  >Schulveranstaltung - Allg. Infos</b-breadcrumb-item
                >
                <b-breadcrumb-item active>Begleitpersonal</b-breadcrumb-item>
              </b-breadcrumb>
            </b-col>
          </b-row>
          <b-row align-h="center">
            <b-col cols="12" md="8">
              <!-- Einfügen der Templates für die Begleitpersonen -->
              <div
                v-for="(escort, index) in escorts.output"
                v-bind:key="escort.id"
              >
                <!-- Begleitformular -->
                <EscortsComp
                  v-bind:escort="escort"
                  v-bind:index="index"
                  v-bind:bsd="escorts.startDate"
                  v-bind:bst="escorts.startTime"
                  v-bind:bed="escorts.endDate"
                  v-bind:bet="escorts.endTime"
                  v-on:startDate="changeStartDate"
                  v-on:endDate="changeEndDate"
                  v-on:startTime="changeStartTime"
                  v-on:endTime="changeEndTime"
                  v-on:selected="changeSelected"
                  v-on:validTime="validateTime"
                  v-on:startadresse="changeStartAdresse"
                  v-on:meetingpoint="changeMeetingPoint"
                />
                <!-- Reiseantragsformular -->
                <TravelApplication
                  v-bind:escort="escort"
                  v-bind:index="index"
                  v-on:update="updateTravel"
                />
              </div>

              <center>
                <!-- Button zum einreichen -->
                <button v-on:click="einreichen" class="blueish-gradiant">
                  Einreichen
                </button>
              </center>
            </b-col>
          </b-row>
        </b-container>
      </b-col>
    </b-row>
  </b-container>
</template>
<script>
import EscortsComp from "@/components/new/EscortsComp.vue";
import TravelApplication from "@/components/new/TravelApplication.vue";
import axios from "axios";

export default {
  name: "NewApplication",
  components: {
    TravelApplication,
    EscortsComp
  },
  props: ["escorts", "user", "url", "token", "refresh_token"],
  data() {
    return {
      travelData: []
    };
  },
  methods: {
    /**
     * Diese Methode ändert die angezeigte Komponente
     * @param component Die neue Komponente, welche angezeigt werden soll
     * @param back Boolean-Wert, ob die neue Komponente in die History des Browsers gespeichert werden soll
     * @param application Die ID des Antrags, welcher angezeigt werden soll
     */
    changeComponent(component, back = true, application = null) {
      this.$emit("change-component", component, back, application);
    },
    /**
     * Diese Methode sorgt dafür, dass nicht unnötigerweise geclickt wird, falls nur makiert worden ist
     */
    checkClick() {
      if (
        window
          .getSelection()
          .toString()
          .trim() === ""
      ) {
        return true;
      } else {
        return false;
      }
    },
    /**
     * Diese Methode leitet den Benutzer auf die Startseite weiter
     */
    index() {
      if (this.checkClick()) {
        this.changeComponent("Index");
      }
    },
    /**
     * Diese Methode aktualisiert die Daten des Reiseformulars
     * @param index Der Index des zu ändernden Eintrags
     * @param data Die Daten zur aktualisierung
     */
    updateTravel(index, data) {
      this.escorts.output[index].degree = data.degree;
      this.escorts.output[index].title = data.title;
      this.escorts.output[index].personalnummer = data.personalnummer;
      this.escorts.output[index].transport = data.transport;
      this.escorts.output[index].ausgangspunkt = data.ausgangspunkt;
      this.escorts.output[index].endpunkt = data.endpunkt;
      this.escorts.output[index].reason1 = data.reason1;
      this.escorts.output[index].reason = data.reason;
      this.escorts.output[index].bonus_meilen = data.bonus_meilen;
      this.escorts.output[index].reisekosten = data.reisekosten;
      this.escorts.output[index].aufenthaltskosten = data.aufenthaltskosten;
      this.escorts.output[index].von = data.von;
      this.escorts.output[index].sonstige_kosten = data.sonstige_kosten;
      this.escorts.output[index].geschaetzte_kosten = data.geschaetzte_kosten;
      if (data.business_karte.includes("too")) {
        this.escorts.output[index].emitted_out = true;
      } else {
        this.escorts.output[index].emitted_out = false;
      }
      if (data.business_karte.includes("back")) {
        this.escorts.output[index].emitted_ret = true;
      } else {
        this.escorts.output[index].emitted_ret = false;
      }
    },
    /**
     * Diese Methode gibt den Wert der Variable als Zahl zurück
     * @param input Die Zahl, welche umgewandelt werden soll
     * @returns Die Zahl des gegebenen Werts
     */
    returnValue(input) {
      if (input === undefined || input === null || input === "") {
        return null;
      } else {
        return Number(input);
      }
    },
    /**
     * Diese Methode gibt den String der Variable zurück
     * @param input Der String, welcher umgewandelt werden soll
     * @returns Den String der gegebenen Variable
     */
    returnString(input) {
      if (input === undefined || input === null) {
        return null;
      } else {
        return input;
      }
    },
    /**
     * Diese Methode gibt den Wert der Variable als Boolean-Wert zurück
     * @param input Der Boolean-Wert als String
     * @returns Der Boolean-Wert der Variable
     */
    returnBoolean(input) {
      if (input === undefined || input === null || input === "") {
        return null;
      } else {
        if (input === "false") return false;
        else return true;
      }
    },
    /**
     * Diese Methode fügt die einzelnen Daten zu einem für das Backend verwendbares Objekt zusammen und sendet dieses an das Backend
     */
    einreichen() {
      if (this.checkClick()) {
        var teachers = [];
        var business = [];
        var invoices = [];
        for (let i = 0; i < this.escorts.output.length; i++) {
          invoices.push({
            id: i,
            degree: this.returnString(this.escorts.output[i].degree),
            title: this.returnString(this.escorts.output[i].title),
            name: this.returnString(this.escorts.output[i].name),
            surname: this.returnString(this.escorts.output[i].surname),
            trip_begin_time: this.createNewDate(
              this.escorts.startDate,
              this.escorts.startTime
            ),
            trip_end_time: this.createNewDate(
              this.escorts.endDate,
              this.escorts.endTime
            ),
            staffnr: this.returnValue(this.escorts.output[i].personalnummer),
            starting_point: this.returnString(this.escorts.start),
            end_point: this.returnString(this.escorts.ziel),
            filing_date: this.createNewDate(
              new Date().toISOString().split("T")[0],
              new Date().toISOString().split("T")[1]
            )
          });
          teachers.push({
            attendance_from: this.createNewDate(
              this.escorts.output[i].startDate,
              this.escorts.output[i].startTime
            ),
            attendance_till: this.createNewDate(
              this.escorts.output[i].endDate,
              this.escorts.output[i].endTime
            ),
            meeting_point: this.returnString(
              this.escorts.output[i].meetingpoint
            ),
            name: this.returnString(
              this.escorts.output[i].name + " " + this.escorts.output[i].surname
            ),
            role: this.returnValue(this.escorts.output[i].role),
            group: this.returnValue(this.escorts.output[i].selected),
            shortname: this.returnString(this.escorts.output[i].shortname),
            start_address: this.returnString(
              this.escorts.output[i].startadresse
            )
          });
          var otherteachers = [];
          for (let j = 0; j < this.escorts.output.length; j++)
            otherteachers.push(this.escorts.output[j].shortname);
          otherteachers.splice(i, 1);
          var bonus1;
          var bonus2;
          if (
            this.escorts.output[i].bonus_meilen[0] === "0" ||
            this.escorts.output[i].bonus_meilen[1] === "0"
          ) {
            bonus1 = true;
          } else {
            bonus1 = false;
          }

          if (
            this.escorts.output[i].bonus_meilen[0] === "1" ||
            this.escorts.output[i].bonus_meilen[1] === "1"
          ) {
            bonus2 = true;
          } else {
            bonus2 = false;
          }

          business.push({
            id: i,
            degree: this.returnString(this.escorts.output[i].degree),
            title: this.returnString(this.escorts.output[i].title),
            name: this.returnString(this.escorts.output[i].name),
            surname: this.returnString(this.escorts.output[i].surname),
            staffnr: this.returnValue(this.escorts.output[i].personalnummer),
            trip_begin_time: this.createNewDate(
              this.escorts.startDate,
              this.escorts.startTime
            ),
            trip_end_time: this.createNewDate(
              this.escorts.endDate,
              this.escorts.endTime
            ),
            service_begin_time: this.createNewDate(
              this.escorts.output[i].startDate,
              this.escorts.output[i].startTime
            ),
            service_end_time: this.createNewDate(
              this.escorts.output[i].endDate,
              this.escorts.output[i].endTime
            ),
            trip_goal: this.returnString(this.escorts.ziel),
            travel_purpose: this.returnString(this.escorts.output[i].reason1),
            travel_mode: this.returnValue(this.escorts.output[i].transport),
            starting_point: this.returnValue(
              this.escorts.output[i].ausgangspunkt
            ),
            end_point: this.returnValue(this.escorts.output[i].endpunkt),
            reasoning: this.returnString(this.escorts.output[i].reason),
            other_participants: otherteachers,
            bonus_mile_confirmation_1: bonus1,
            bonus_mile_confirmation_2: bonus2,
            travel_costs_paid_by_someone: this.returnBoolean(
              this.escorts.output[i].reisekosten
            ),
            staying_costs_paid_by_someone: this.returnBoolean(
              this.escorts.output[i].aufenthaltskosten
            ),
            paid_by_whom: this.returnString(this.escorts.output[i].von),
            other_costs: this.returnValue(
              this.escorts.output[i].sonstige_kosten
            ),
            estimated_costs: this.returnValue(
              this.escorts.output[i].geschaetzte_kosten
            ),
            date_application_filed: this.createNewDate(
              new Date().toISOString().split("T")[0],
              new Date().toISOString().split("T")[1]
            ),
            business_card_emitted_outward: this.escorts.output[i].emitted_out,
            business_card_emitted_return: this.escorts.output[i].emitted_ret
          });
        }
        var data = {
          uuid: this.returnString(""),
          name: this.returnString(this.escorts.description),
          kind: 0,
          miscellaneous_reason: this.returnString(""),
          progress: 1,
          start_time: this.createNewDate(
            this.escorts.startDate,
            this.escorts.startTime
          ),
          end_time: this.createNewDate(
            this.escorts.endDate,
            this.escorts.endTime
          ),
          notes: this.returnString(this.escorts.notes),
          start_address: this.returnString(this.escorts.start),
          destination_address: this.returnString(this.escorts.ziel),
          school_event_details: {
            classes: this.escorts.class,
            amount_male_students: this.escorts.count_student_male,
            amount_female_students: this.escorts.count_student_female,
            duration_in_days: this.returnValue(this.escorts.exkursLength),
            teachers: teachers
          },
          business_trip_applications: business,
          travel_invoices: invoices
        };
        axios
          .post(this.url + "/createApplication", data, {
            headers: {
              Authorization: "Basic " + this.token
            }
          })
          .then(response => {
            response.toString();
            this.createConfirm();
            this.changeComponent("Index");
          })
          .catch(error => {
            switch (error.response.status) {
              case 401:
                axios
                  .post(this.url + "/login/refresh", {
                    refresh_token: this.refresh_token
                  })
                  .then(resp => {
                    this.$emit(
                      "updateToken",
                      resp.data.access_token,
                      resp.data.refresh_token
                    );
                    axios
                      .post(this.url + "/createApplication", data, {
                        headers: {
                          Authorization: "Basic " + resp.data.access_token
                        }
                      })
                      .then(res => {
                        res.toString();
                        this.createConfirm();
                        this.changeComponent("Index");
                      })
                      .catch(e => {
                        e.toString();
                        this.failedConfirm();
                      });
                  })
                  .catch(err => {
                    err.toString();
                    this.$emit("logout");
                  });
                break;
              default:
                this.failedConfirm();
                break;
            }
          });
      }
    },
    /**
     * Diese Methode aktualisiert das StartDatum einer Begleitperson
     * @param index Der Index der Begleitperson
     * @param newStartDate Das neue Startdatum
     */
    changeStartDate(index, newStartDate) {
      this.escorts.output[index].startDate = newStartDate;
    },
    /**
     * Erstellt ein neues Datum, welches im richtigen Datenformat ist
     */
    createNewDate(date, time) {
      var tmp = new Date(date + "T" + time);
      return tmp.toISOString();
    },
    /**
     * Diese Methode zeigt dem Benutzer an, dass der Antrag erfolgreich gespeichert worden ist
     */
    createConfirm() {
      this.$bvToast.toast("Antrag erstellt!", {
        title: "Antrag wurde erfolgreich erstellt",
        autoHideDelay: 2500,
        appendToast: false,
        variant: "success"
      });
    },
    /**
     * Diese Methode meldet den Benutzer vom System ab
     */
    logout() {
      if (this.checkClick()) {
        this.$emit("logout");
      }
    },
    /**
     * Diese Methode leitet den Benutzer auf die AccountView-Seite weiter
     */
    accountView() {
      if (this.checkClick()) {
        this.changeComponent("AccountView");
      }
    },
    /**
     * Diese Methode zeigt dem Benutzer an, dass der Antrag erfolgreich gespeichert worden ist
     */
    failedConfirm() {
      this.$bvToast.toast("Es ist ein Fehler aufgetreten!", {
        title: "Antrag wurden nicht erstellt",
        autoHideDelay: 2500,
        appendToast: false,
        variant: "danger"
      });
    },
    /**
     * Diese Methode aktualisiert das EndDatum einer Begleitperson
     * @param index Der Index der Begleitperson
     * @param newEndDate Das neue Enddatum
     */
    changeEndDate(index, newEndDate) {
      this.escorts.output[index].endDate = newEndDate;
    },
    /**
     * Diese Methode aktualisiert die Startzeit einer Begleitperson
     * @param index Der Index der Begleitperson
     * @param newStartTime Die neue Startzeit
     */
    changeStartTime(index, newStartTime) {
      this.escorts.output[index].startTime = newStartTime;
    },
    /**
     * Diese Methode aktualisiert die Endzeit einer Begleitperson
     * @param index Der Index der Begleitperson
     * @param newEndTime Die neue Endzeit
     */
    changeEndTime(index, newEndTime) {
      this.escorts.output[index].endTime = newEndTime;
    },
    /**
     * Diese Methode aktualisiert die Lehrergruppe einer Begleitperson
     * @param index Der Index der Begleitperson
     * @param newSelected Die neue Lehrergruppe
     */
    changeSelected(index, newSelected) {
      this.escorts.output[index].selected = newSelected;
    },
    /**
     * Diese Methode aktualisiert die Startadresse einer Begleitperson
     * @param index Der Index der Begleitperson
     * @param newStartAdresse Die neue Startadresse
     */
    changeStartAdresse(index, newStartAdresse) {
      this.escorts.output[index].startadresse = newStartAdresse;
    },
    /**
     * Diese Methode aktualisiert den MeetingPoint einer Begleitperson
     * @param index Der Index der Begleitperson
     * @param newStartAdresse Der neue MeetingPoint
     */
    changeMeetingPoint(index, newMeetingPoint) {
      this.escorts.output[index].meetingpoint = newMeetingPoint;
    },
    /**
     * Diese Methode setzt die Variable der validen Zeit
     * @param newValidTime Boolean-Wert, ob die eingegebene Zeit eine valide ist
     */
    validateTime(newValidTime) {
      this.validTime = newValidTime;
    },
    /**
     * Diese Methode leitet den Benutzer auf die School-Seite weiter
     */
    school() {
      if (this.checkClick()) {
        this.changeComponent("School");
      }
    },
    /**
     * Diese Methode leitet den Benutzer auf die NewApplication-Seite weiter
     */
    uebersicht() {
      if (this.checkClick()) {
        this.changeComponent("NewApplication");
      }
    }
  },
  mounted() {
    for (let i = 0; i < this.escorts.output.length; i++) {
      (this.escorts.output[i].degree = null),
        (this.escorts.output[i].title = null),
        (this.escorts.output[i].personalnummer = null);
      this.escorts.output[i].transport = null;
      this.escorts.output[i].ausgangspunkt = null;
      this.escorts.output[i].endpunkt = null;
      this.escorts.output[i].reason1 = null;
      this.escorts.output[i].reason = null;
      this.escorts.output[i].bonus_meilen = [];
      this.escorts.output[i].reisekosten = null;
      this.escorts.output[i].aufenthaltskosten = null;
      this.escorts.output[i].von = null;
      this.escorts.output[i].sonstige_kosten = null;
      this.escorts.output[i].geschaetzte_kosten = null;
    }
  }
};
</script>
