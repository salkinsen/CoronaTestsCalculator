<template>
  <div id="app">
    <b-container>
      <b-row align-h="center" class="mb-3">
        <b-card>
          <b-col>
            <b-form>
              <b-form-group
                id="incidence-group"
                label="Inzidenz (pro 100.000)"
                label-for="incidence"
              >
                <b-form-input
                  id="incidence"
                  type="number"
                  v-model="incidence"
                ></b-form-input>
              </b-form-group>
              <b-form-group
                id="darkfigure-group"
                label="Faktor Dunkelziffer"
                label-for="darkfigure"
              >
                <b-form-input
                  id="darkfigure"
                  type="number"
                  v-model="darkfigure"
                ></b-form-input>
              </b-form-group>
              <b-form-group
                id="sensitivity-group"
                label="Sensitivität in % (nicht Spezifität!)"
                label-for="sensitivity"
              >
                <b-form-input
                  id="sensitivity"
                  type="number"
                  v-model="sensitivity"
                ></b-form-input>
              </b-form-group>
              <b-form-group
                id="people-group"
                label="Anzahl Personen"
                label-for="people"
              >
                <b-form-input
                  id="people"
                  type="number"
                  v-model="people"
                ></b-form-input>
              </b-form-group>
            </b-form>
          </b-col>
        </b-card>
      </b-row>
      <b-row class="mb-2">
        <b-col
          ><b-card class="text-center" title="Szenario 1">
            <div>{{ people }} Leute treffen sich ohne Tests.</div></b-card
          >
        </b-col>
        <b-col
          ><b-card class="text-center" title="Szenario 2">
            <div>
              {{ people }} Leute treffen sich, nachdem sie alle negativ getestet
              wurden.
            </div>
          </b-card>
        </b-col>
      </b-row>
      <b-row class="mb-2" align-h="center">
        <b-col
          ><b-card>
            <b-table-simple hover>
              <b-thead head-variant="dark">
                <b-tr>
                  <b-th></b-th>
                  <b-th>Szenario 1</b-th>
                  <b-th>Szenario 2</b-th>
                </b-tr>
              </b-thead>
              <b-tbody>
                <b-tr>
                  <b-td
                    >Wahrscheinlichkeit, dass mind. 1 Person infiziert
                    ist:</b-td
                  >
                  <b-td>{{ scenario1prob }} %</b-td>
                  <b-td>{{scenario2prob}} %</b-td>
                </b-tr>
              </b-tbody>
            </b-table-simple>
          </b-card>
        </b-col>
      </b-row>
      <b-row class="mb-2" align-h="center">
        <b-col
          ><b-card class="text-center">
            Szenario 2 reduziert das Risiko um den Faktor {{scenario1prob / scenario2prob}}.
          </b-card>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      incidence: 70,
      sensitivity: 80,
      people: 6,
      darkfigure: 4,
    }
  },
  computed: {
      scenario1prob : function() {
        var prop1PersonHealthy = 1 - ((this.incidence / 100000) * this.darkfigure);
        var propAllHealthy = Math.pow(prop1PersonHealthy, this.people);
        return (1 - propAllHealthy) * 100;
      },
      scenario2prob : function() {
        var falseNegProb = (100 - this.sensitivity) / 100;
        var prop1PersonHealthy = 1 - ((this.incidence / 100000) * this.darkfigure * falseNegProb);
        var propAllHealthy = Math.pow(prop1PersonHealthy, this.people);
        return (1 - propAllHealthy) * 100;
      }
  }
}
</script>
