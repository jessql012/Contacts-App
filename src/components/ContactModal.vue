<template>
  <v-dialog :id="modalId" v-model="open" width="600">   
    <template v-slot:activator="{ on, attrs }">
      <v-btn
        class="activate-button"
        :color="theme"
        outlined
        v-bind="attrs"
        v-on="on"
      >More Details
      </v-btn>

    </template>
    <v-card>
      <v-card-title class="card-title">
        <span>{{person.name}} </span>
        <v-spacer></v-spacer>
        <v-btn icon @click=" open = false" class="close-button">
        <v-icon>mdi-close</v-icon>
        </v-btn>
      </v-card-title>
      <v-divider></v-divider>
      <v-card-text class="mt-3">
        <b>Username: </b>{{ person.username }} <br>
        <b>Email: </b>{{ person.email }} <br>
        <b>Phone: </b>{{ person.phone }} <br>
        <b>Website: </b>{{ person.website }} <br>
        <b>Company: </b>{{ person.company.name }} <br>
        <b>Company Catchphrase: </b>{{ person.company.catchPhrase }} <br>
        <b>Company BS: </b>{{ person.company.bs }} <br>
        <b>Address: </b>{{person.address.suite}}, {{person.address.street}}, {{person.address.city}}, {{person.address.zipcode}}
      </v-card-text>
    </v-card>
  </v-dialog>
</template>

<script>
export default {
    name: "ContactModal",
    props: ['person', 'theme'],
    data(){
        return{
            map: null,
            open: false,
            modalId: 'modal' + this.person.id
        }
    }
}
</script>

<style scoped>
.card-title {
    display: flex;
}

.activate-button {
    width: 100%;
}
.close-button {
    align-self: flex-end;
    right: 0;
}
</style>