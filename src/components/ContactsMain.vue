<template>
  <v-main class="flex">
      <v-app-bar :color='theme' prominent dark elevation="4">
        <v-row class="v-app-bar-title">
          <v-icon size="60" color="white">mdi-email</v-icon>
          <h1><b>CONTACTS</b></h1>
        </v-row>    
        <v-spacer></v-spacer>
        <v-subheader class="v-app-bar-subtitle">Number of Contacts: {{numContacts}}</v-subheader>
      </v-app-bar>

      <v-container class="body">
        <v-autocomplete
          v-model="search"
          :items="contacts"
          :color="theme"
          item-text = "name"
          item-value = "id"
          auto-select-first
          append-outer-icon="mdi-magnify"
          hide-no-data
          hide-selected
          allow-overflow
          placeholder="Start typing to search for a contact..."
          return-object
          @change="onSearch"
          @click:append-outer="onSearch"
        ></v-autocomplete>
        <v-alert v-model="showAlert" border="left" :color="theme" dark dismissible>
          Contact not found.
        </v-alert>
        <div>
          <v-container fluid id='deck'>
            <v-card 
              class="card" 
              elevation="2" 
              v-for="person in contacts" 
              :key="person.id" 
              v-bind:id="person.id"
            >
              <v-container class="card-contents">
                <v-card-title class="mb-2">
                  {{person.name}}
                </v-card-title>
                <v-card-text>
                  <b>Email: </b>{{ person.email }} <br>
                  <b>Phone: </b>{{ person.phone }} <br>
                  <b>Website: </b>{{ person.website }} <br>
                </v-card-text>
                <ContactModal :person="person" :theme="theme" />
              </v-container>
            </v-card>
          </v-container>
        </div>
      </v-container>
    </v-main>
</template>

<script>
import axios from 'axios';
import ContactModal from './ContactModal'

export default{
    name: "ContactsMain",
    components: {
        ContactModal
    },
    data(){
        return{
            contacts: [],
            numContacts: 0,
            search: null,
            showAlert: false,
            theme: 'light-green darken-2',
        }
    },
    methods: {
        async getJSON () {
            await axios.get('https://jsonplaceholder.typicode.com/users')
            .then((getResponse) => {
                for (let i = 0; i < getResponse.data.length; i++) {
                    this.contacts.push(getResponse.data[i]);
                }
                
                // Sort alphabetically
                this.contacts = this.contacts.sort((a, b) =>
                    a.name.localeCompare(b.name, 'en', { sensitivity: 'base' })
                );
            }).catch(error => error) 

            this.numContacts = this.contacts.length;
        },
        onSearch() {
            if (this.search == null) {
                this.showAlert = true
            }
            else {
                this.showAlert = false
                // Set a timer for the animation
                const delay = ms => new Promise(result => setTimeout(result, ms));

                var deck = document.getElementById('deck');
                for (let i = 0; i < deck.children.length; i++) {
                    if (deck.children[i].id == this.search.id) {
                        deck.children[i].scrollIntoView({behavior: 'smooth', block: 'center'}); // Scroll to card
                        deck.children[i].classList.add('expand') // Play animation
                        delay(2000).then(() => {
                            deck.children[i].classList.remove('expand')});
                    }
                }
            }
        }
    },
    mounted () {
        this.getJSON();
    }
};
</script>

<style scoped>

.expand {
    animation: expand 1s ;
}

@keyframes expand {
    10% {
        transform: scale(1);
    }

    20% {
        transform: scale(1);
    }

    50% {
        transform: scale(1.1);
    }

    90% {
        transform: scale(1);
    }
    }

.v-app-bar-title {
    padding-left: 2rem;
    margin: auto;
    position: absolute;
    align-self: center;
    flex-wrap: wrap;
    align-items: stretch;
    column-gap: 1rem;
}

.v-app-bar-subtitle {
    vertical-align: bottom;
    flex-wrap: wrap;
    position: relative;
}

.body {
    width: 80%;
}

.search{
    padding: 2rem;
    width: 100%;
}

#deck{
    align-items: center;
    display: flex;
    flex-direction: column;
    row-gap: 2rem;
}

.card{
    display: flex;
    flex-direction: column;
    width: 100%;
}

.card-contents{
    width: 100%;
    height: inherit;
}

</style>