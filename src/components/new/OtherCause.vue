<!-- Template für Fortbildungen etc. -->
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
              <!-- Verzeichnis Anzeige -->
              <b-breadcrumb style="background-color: white">
                <b-breadcrumb-item v-on:click="uebersicht"
                  >Antrag Übersicht</b-breadcrumb-item
                >
                <b-breadcrumb-item active>Anderer Grund</b-breadcrumb-item>
              </b-breadcrumb>
            </b-col>
          </b-row>
          <b-row align-h="center">
            <b-col cols="12" md="8">
              <!-- Startdatum -->
              <b-form-group
                id="startd"
                label-cols-sm="4"
                label-cols-lg="3"
                content-cols-sm
                content-cols-lg="7"
                description="Geben Sie das Startdatum der Fortbildung ein."
                label="Startdatum"
                label-for="std"
              >
                <b-form-datepicker
                  id="std"
                  v-model="startDate"
                  :state="Time"
                  v-on:input="checkTime"
                  class="mb-2"
                  placeholder="Datum auswählen"
                ></b-form-datepicker>
                <b-form-invalid-feedback id="std-feedback">
                  Start der Fortbildung muss vor dem Ende der Fortbildung sein!
                </b-form-invalid-feedback>
              </b-form-group>
              <!-- Startzeit -->
              <b-form-group
                id="startz"
                label-cols-sm="4"
                label-cols-lg="3"
                content-cols-sm
                content-cols-lg="7"
                description="Geben Sie die Startzeit der Fortbildung ein."
                label="Startzeit"
                label-for="stz"
              >
                <b-form-timepicker
                  id="stz"
                  v-model="startTime"
                  :state="Time"
                  v-on:input="checkTime"
                  locale="de"
                  placeholder="Zeit auswählen"
                ></b-form-timepicker>
                <b-form-invalid-feedback id="stz-feedback">
                  Start der Fortbildung muss vor dem Ende der Fortbildung sein!
                </b-form-invalid-feedback>
              </b-form-group>
              <!-- Enddatum -->
              <b-form-group
                id="endd"
                label-cols-sm="4"
                label-cols-lg="3"
                content-cols-sm
                content-cols-lg="7"
                description="Geben Sie das Enddatum der Fortbildung ein."
                label="Enddatum"
                label-for="end"
              >
                <b-form-datepicker
                  id="end"
                  v-model="endDate"
                  :state="Time"
                  v-on:input="checkTime"
                  class="mb-2"
                  placeholder="Datum auswählen"
                ></b-form-datepicker>
                <b-form-invalid-feedback id="end-feedback">
                  Start der Fortbildung muss vor dem Ende der Fortbildung sein!
                </b-form-invalid-feedback>
              </b-form-group>
              <!-- Endzeit -->
              <b-form-group
                id="endz"
                label-cols-sm="4"
                label-cols-lg="3"
                content-cols-sm
                content-cols-lg="7"
                description="Geben Sie die Endzeit der Fortbildung ein."
                label="Endzeit"
                label-for="enz"
              >
                <b-form-timepicker
                  id="enz"
                  v-model="endTime"
                  :state="Time"
                  v-on:input="checkTime"
                  locale="de"
                  placeholder="Zeit auswählen"
                ></b-form-timepicker>
                <b-form-invalid-feedback id="enz-feedback">
                  Start der Fortbildung muss vor dem Ende der Fortbildung sein!
                </b-form-invalid-feedback>
              </b-form-group>
              <!-- Startadresse -->
              <b-form-group
                id="start"
                label-cols-sm="4"
                label-cols-lg="3"
                content-cols-sm
                content-cols-lg="7"
                description="Geben Sie die Startadresse ein."
                label="Startadresse"
                label-for="sa"
              >
                <b-form-input
                  id="sa"
                  v-model="start"
                  :state="Start"
                  v-on:input="checkStart"
                  list="startadd"
                ></b-form-input>
                <datalist id="startadd">
                  <option>Wexstraße 19-23, 1200 Wien, Österreich</option>
                </datalist>
                <b-form-invalid-feedback id="sa-feedback">
                  Keine Startadresse angegeben!
                </b-form-invalid-feedback>
              </b-form-group>
              <!-- Zieladresse -->
              <b-form-group
                id="end"
                label-cols-sm="4"
                label-cols-lg="3"
                content-cols-sm
                content-cols-lg="7"
                description="Geben Sie die Zieladresse des Antrags ein."
                label="Zieladresse"
                label-for="ea"
              >
                <b-form-input
                  id="ea"
                  v-model="end"
                  :state="End"
                  v-on:input="checkEnd"
                ></b-form-input>
                <b-form-invalid-feedback id="ea-feedback">
                  Keine Zieladresse angegeben!
                </b-form-invalid-feedback>
              </b-form-group>
              <!-- Grund der Fortbildung -->
              <b-form-group
                id="grund"
                label-cols-sm="4"
                label-cols-lg="3"
                content-cols-sm
                content-cols-lg="7"
                description="Wählen Sie Ihren Grund aus."
                label="Gründe"
                label-for="gr"
              >
                <b-form-radio-group
                  id="gr"
                  v-model="selected"
                  :options="options"
                  v-on:input="checkSelected"
                  class="mb-3"
                  value-field="item"
                  text-field="name"
                  disabled-field="notEnabled"
                ></b-form-radio-group>
              </b-form-group>
              <!-- Auswahl des sonstigen Grundes -->
              <b-form-group
                id="sonst"
                label-cols-sm="4"
                label-cols-lg="3"
                content-cols-sm
                content-cols-lg="7"
                description="Geben Sie den Grund an."
                label="Sonstiger Grund"
                label-for="son"
                v-if="selected == '10'"
              >
                <b-form-input
                  id="son"
                  v-model="son"
                  :state="Sonstiges"
                  v-on:input="checkSonstiges"
                >
                </b-form-input>
                <b-form-invalid-feedback id="sonst-feedback">
                  Kein Sonstiger Grund angegeben!
                </b-form-invalid-feedback>
              </b-form-group>
              <!-- Titel des Dienstauftrages, falls dieser als Grund gewählt wurde -->
              <b-form-group
                id="titel"
                label-cols-sm="4"
                label-cols-lg="3"
                content-cols-sm
                content-cols-lg="7"
                description="Geben Sie den Titel des Dienstauftrages ein."
                label="Titel"
                label-for="tit"
                v-if="selected == '8'"
              >
                <b-form-input
                  id="tit"
                  v-model="title"
                  :state="Titel"
                  v-on:input="checkTitel"
                ></b-form-input>
                <b-form-invalid-feedback id="titel-feedback">
                  Kein Titel angegeben!
                </b-form-invalid-feedback>
              </b-form-group>
              <!-- GZ des Dienstauftrages, falls dieser als Grund gewählt wurde -->
              <b-form-group
                id="gz"
                label-cols-sm="4"
                label-cols-lg="3"
                content-cols-sm
                content-cols-lg="7"
                description="Geben Sie die GZ des Dienstauftrages ein."
                label="GZ"
                label-for="gzn"
                v-if="selected == '8'"
              >
                <b-form-input
                  id="gzn"
                  type="number"
                  v-model="gz"
                  :state="GZset"
                  v-on:input="checkGZ"
                ></b-form-input>
                <b-form-invalid-feedback id="gzn-feedback">
                  Keine GZ angegeben!
                </b-form-invalid-feedback>
              </b-form-group>
              <!-- Zusätzliche Anmerkungen -->
              <b-form-group
                id="anmerkung"
                label-cols-sm="4"
                label-cols-lg="3"
                content-cols-sm
                content-cols-lg="7"
                description="Geben Sie zusätzliche Anmerkungen an."
                label="Anmerkungen"
                label-for="an"
              >
                <b-form-textarea
                  id="an"
                  placeholder="Anmerkungen"
                  rows="3"
                  no-resize
                  v-model="notes"
                ></b-form-textarea>
              </b-form-group>
              <!-- Reiseanträge -->
              <TravelApplication
                v-if="selected === '8' || selected === '10'"
                v-bind:escort="escort"
                v-bind:index="1"
                v-on:update="updateTravel"
              />
              <center>
                <!-- Button zum einreichen -->
                <button v-on:click="einreichen()" class="blueish-gradiant">
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
import axios from "axios";
import TravelApplication from "@/components/new/TravelApplication.vue";
export default {
  components: {
    TravelApplication
  },
  name: "NewApplication",
  props: ["url", "user", "token", "refresh_token"],
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
     * Diese Methode leitet den Benutzer auf die Startseite weiter
     */
    index() {
      if (this.checkClick()) {
        this.changeComponent("Index");
      }
    },
    /**
     * Diese Methode überprüft die eingegebenen Daten, ob diese einen sinnvollen Wert haben
     */
    checkTime() {
      if (
        this.startDate !== "" &&
        this.startTime !== "" &&
        this.endDate !== "" &&
        this.endTime !== ""
      ) {
        let start = new Date(this.startDate + "T" + this.startTime);
        let end = new Date(this.endDate + "T" + this.endTime);
        if (end - start <= 0) {
          this.Time = false;
        } else {
          this.Time = true;
        }
        this.checkInputs();
      }
    },
    /**
     * Diese Methode überprüft, ob eine Art von Antrag ausgewählt worden ist
     */
    checkSelected() {
      if (this.selected !== "") {
        this.isSelected = true;
      } else {
        this.isSelected = false;
      }
      this.checkInputs();
    },
    /**
     * Diese Methode überprüft, ob der Titel des Dienstauftrags eingegeben worden ist
     */
    checkTitel() {
      if (this.title === "") {
        this.Titel = false;
      } else {
        this.Titel = true;
      }
      this.checkInputs();
    },
    /**
     * Diese Methode überprüft, ob die GZ des Diesntauftrages eingegeben worden ist
     */
    checkGZ() {
      if (this.gz === "") {
        this.GZset = false;
      } else {
        this.GZset = true;
      }
      this.checkInputs();
    },
    /**
     * Diese Methode überprüft, ob ein Sonstiger Grund eingegeben worden ist
     */
    checkSonstiges() {
      if (this.son === "") {
        this.Sonstiges = false;
      } else {
        this.Sonstiges = true;
      }
      this.checkInputs();
    },
    /**
     * Diese Methode überprüft, ob die Eingaben valide sind
     */
    checkInputs() {
      switch (this.selected) {
        case "B":
          if (
            this.Time === true &&
            this.Titel === true &&
            this.GZset === true
          ) {
            this.validInputs = true;
          } else {
            this.validInputs = false;
          }
          break;
        case "D":
          if (this.Time === true && this.Sonstiges === true) {
            this.validInputs = true;
          } else {
            this.validInputs = false;
          }
          break;
        default:
          if (this.Time === true && this.isSelected === true) {
            this.validInputs = true;
          } else {
            this.validInputs = false;
          }
          break;
      }
    },
    /**
     * Diese Methode leitet den Benutzer auf die Others-Seite weiter
     */
    workshop() {
      if (this.checkClick()) {
        this.changeComponent("Others");
      }
    },
    /**
     * Diese Methode leitet den Benutzer auf die NewApplication-Seite weiter
     */
    uebersicht() {
      if (this.checkClick()) {
        this.changeComponent("NewApplication");
      }
    },
    /**
     * Diese Methode überprüft, ob die Zieladresse eingegeben worden ist
     */
    checkEnd() {
      if (this.end === "") {
        this.End = false;
      } else {
        this.End = true;
      }
      this.checkInputs();
    },
    /**
     * Diese Methode überprüft, ob die Startadresse eingegeben worden ist
     */
    checkStart() {
      if (this.start === "") {
        this.Start = false;
      } else {
        this.Start = true;
      }
      this.checkInputs();
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
     * Diese Methode aktualisiert die Daten des Reiseformulars
     * @param index Der Index des zu ändernden Eintrags
     * @param data Die Daten zur aktualisierung
     */
    updateTravel(index, data) {
      index.toString();
      this.teacher.degree = data.degree;
      this.teacher.title = data.title;
      this.teacher.personalnummer = data.personalnummer;
      this.teacher.transport = data.transport;
      this.teacher.ausgangspunkt = data.ausgangspunkt;
      this.teacher.endpunkt = data.endpunkt;
      this.teacher.reason1 = data.reason1;
      this.teacher.reason = data.reason;
      this.teacher.bonus_meilen = data.bonus_meilen;
      this.teacher.reisekosten = data.reisekosten;
      this.teacher.aufenthaltskosten = data.aufenthaltskosten;
      this.teacher.von = data.von;
      this.teacher.sonstige_kosten = data.sonstige_kosten;
      this.teacher.geschaetzte_kosten = data.geschaetzte_kosten;
      if (data.business_karte.includes("too")) {
        this.teacher.emitted_out = true;
      } else {
        this.teacher.emitted_out = false;
      }
      if (data.business_karte.includes("back")) {
        this.teacher.emitted_ret = true;
      } else {
        this.teacher.emitted_ret = false;
      }
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
     * Diese Methode fügt die einzelnen Daten zu einem für das Backend verwendbares Objekt zusammen und sendet dieses an das Backend
     */
    einreichen() {
      if (this.checkClick()) {
        if (this.validInputs) {
          if (this.selected !== "7" && this.selected !== "9") {
            var bonus1 = false;
            var bonus2 = false;
            if (this.teacher.bonus_meilen.includes("0")) {
              bonus1 = true;
            } else {
              bonus1 = false;
            }
            if (this.teacher.bonus_meilen.includes("1")) {
              bonus2 = true;
            } else {
              bonus2 = false;
            }
          }
          var business = [];
          var invoice = [];
          var titel;
          switch (this.selected) {
            case "7":
              titel = "Pflegefreistellung";
              break;
            case "8":
              titel = "Dienstauftrag";
              break;
            case "9":
              titel = "Arzttermin";
              break;
            case "10":
              titel = "Sonstiger Antrag";
              break;
            default:
              break;
          }
          if (this.selected !== "7" && this.selected !== "9") {
            business.push({
              id: 0,
              name: this.returnString(this.user.longname.split(" ")[0]),
              surname: this.returnString(this.user.longname.split(" ")[1]),
              degree: this.returnString(this.teacher.degree),
              title: this.returnString(this.teacher.title),
              staffnr: this.returnValue(this.teacher.personalnummer),
              trip_begin_time: this.createNewDate(
                this.startDate,
                this.startTime
              ),
              trip_end_time: this.createNewDate(this.endDate, this.endTime),
              service_begin_time: this.createNewDate(
                this.startDate,
                this.startTime
              ),
              service_end_time: this.createNewDate(this.endDate, this.endTime),
              trip_goal: this.returnString(this.start),
              travel_purpose: this.returnString(this.teacher.reason1),
              travel_mode: this.returnValue(this.teacher.transport),
              starting_point: this.returnValue(this.teacher.ausgangspunkt),
              end_point: this.returnValue(this.teacher.endpunkt),
              reasoning: this.returnString(this.teacher.reason),
              other_participants: [],
              bonus_mile_confirmation_1: bonus1,
              bonus_mile_confirmation_2: bonus2,
              travel_costs_paid_by_someone: this.returnBoolean(
                this.teacher.reisekosten
              ),
              staying_costs_paid_by_someone: this.returnBoolean(
                this.teacher.aufenthaltskosten
              ),
              paid_by_whom: this.returnString(this.teacher.von),
              other_costs: this.returnValue(this.teacher.sonstige_kosten),
              estimated_costs: this.returnValue(
                this.teacher.geschaetzte_kosten
              ),
              date_application_filed: this.createNewDate(
                new Date().toISOString().split("T")[0],
                new Date().toISOString().split("T")[1]
              ),
              business_card_emitted_outward: this.teacher.emitted_out,
              business_card_emitted_return: this.teacher.emitted_ret
            });
            invoice.push({
              id: 0,
              name: this.returnString(this.user.longname.split(" ")[0]),
              surname: this.returnString(this.user.longname.split(" ")[1]),
              degree: this.returnString(this.teacher.degree),
              title: this.returnString(this.teacher.title),
              trip_begin_time: this.createNewDate(
                this.startDate,
                this.startTime
              ),
              trip_end_time: this.createNewDate(this.endDate, this.endTime),
              staffnr: this.returnValue(this.teacher.personalnummer),
              starting_point: this.returnString(this.start),
              end_point: this.returnString(this.end),
              filing_date: this.createNewDate(
                new Date().toISOString().split("T")[0],
                new Date().toISOString().split("T")[1]
              )
            });
          } else {
            business.push({
              id: 0,
              name: this.returnString(this.user.longname.split(" ")[0]),
              surname: this.returnString(this.user.longname.split(" ")[1]),
              trip_begin_time: this.createNewDate(
                this.startDate,
                this.startTime
              ),
              trip_end_time: this.createNewDate(this.endDate, this.endTime),
              service_begin_time: this.createNewDate(
                this.startDate,
                this.startTime
              ),
              service_end_time: this.createNewDate(this.endDate, this.endTime),
              trip_goal: this.returnString(this.start),
              other_participants: [],
              date_application_filed: this.createNewDate(
                new Date().toISOString().split("T")[0],
                new Date().toISOString().split("T")[1]
              )
            });
            invoice.push({
              id: 0,
              name: this.returnString(this.user.longname.split(" ")[0]),
              surname: this.returnString(this.user.longname.split(" ")[1]),
              trip_begin_time: this.createNewDate(
                this.startDate,
                this.startTime
              ),
              trip_end_time: this.createNewDate(this.endDate, this.endTime),
              starting_point: this.returnString(this.start),
              end_point: this.returnString(this.end),
              filing_date: this.createNewDate(
                new Date().toISOString().split("T")[0],
                new Date().toISOString().split("T")[1]
              )
            });
          }
          var data = {
            uuid: this.returnString(""),
            name: this.returnString(titel),
            kind: 6,
            miscellaneous_reason: this.returnString(this.son),
            progress: 1,
            start_time: this.createNewDate(this.startDate, this.startTime),
            end_time: this.createNewDate(this.endDate, this.endTime),
            notes: this.returnString(this.notes),
            start_address: this.returnString(this.end),
            destination_address: this.returnString(this.start),
            last_changed: this.createNewDate(
              new Date().toISOString().split("T")[0],
              new Date().toISOString().split("T")[1]
            ),
            other_reason_details: {
              kind: this.returnValue(this.selected),
              miscellaneous_reason: this.returnString(this.son),
              service_mandate_gz: this.returnValue(this.gz),
              service_mandate_title: this.returnString(this.title),
              filer: this.returnString(this.user.longname)
            },
            business_trip_applications: business,
            travel_invoices: invoice
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
        } else {
          if (this.Time === null) this.Time = false;
          if (this.Titel === null) this.Titel = false;
          if (this.GZset === null) this.GZset = false;
          if (this.Sonstiges === null) this.Sonstiges = false;
        }
      }
    }
  },
  data() {
    return {
      selected: "",
      options: [
        { item: "7", name: "Pflegefreistellung" },
        { item: "8", name: "Dienstauftrag" },
        { item: "9", name: "Arzttermin" },
        { item: "10", name: "Sonstiges" }
      ],
      validInputs: false,
      Time: null,
      isSelected: null,
      Titel: null,
      GZset: null,
      Sonstiges: null,
      Start: null,
      End: null,
      startDate: "",
      startTime: "",
      endDate: "",
      endTime: "",
      title: "",
      gz: "",
      son: "",
      notes: "",
      start: "",
      end: "",
      escort: Object,
      teacher: Object
    };
  }
};
</script>
