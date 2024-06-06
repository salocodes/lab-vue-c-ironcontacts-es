<script setup>
import { ref } from "vue";
import jasonContacts from "./contacts.json";

const contacts = 5;

const showedContacts = ref(jasonContacts.slice(0, contacts));

let nonshowedContacts = jasonContacts.filter(
  (contact, index) => index > contacts
);

const extractRandomContact = () => {
  const remainingContactsNum = nonshowedContacts.length;
  if (remainingContactsNum < 1) {
    alert("No more contacts to display");
    return;
  }

  const randomIndex = Math.floor(Math.random() * remainingContactsNum);
  const randomContact = nonshowedContacts[randomIndex];

  nonshowedContacts = nonshowedContacts.filter(
    (contact, index) => index !== randomIndex
  );

  showedContacts.value.push(randomContact);
};

const restoreContact = (id) => {
  const contact = showedContacts.value.find((contact) => contact.id === id);
  nonshowedContacts.push(contact);

  showedContacts.value = showedContacts.value.filter(
    (contact) => contact.id !== id
  );
};

const sortByPopularity = () => {
  showedContacts.value.sort((a, b) => b.popularity - a.popularity);
};

const sortByName = () => {
  showedContacts.value.sort((a, b) => a.name.localeCompare(b.name));
};
//
</script>

<template>
 
  <main>
    <div class="actions-bar">
      <div class="actions-section">
        <button @click="extractRandomContact" class="plain-button">+</button>
      </div>
      <div class="actions-section">
        <span>Sort by:</span>
        <button @click="sortByPopularity" class="plain-button">
          popularity
        </button>
        <button @click="sortByName" class="plain-button">name</button>
      </div>
    </div>
    <table class="contacts-table" v-if="showedContacts.length > 0">
      <thead>
        <tr>
          <th>Picture</th>
          <th>Name</th>
          <th>Popularity</th>
          <th>Won Oscar</th>
          <th>Won Emmy</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="contact in showedContacts" :key="contact.id">
          <td class="profile-picture-cell">
            <img
              :src="contact.pictureUrl"
              :alt="`${contact.name}'s profile picture`"
              class="profile-picture"
            />
          </td>
          <td>{{ contact.name }}</td>
          <td>{{ contact.popularity.toFixed(2) }}</td>
          <td>{{ contact.wonOscar ? "🏆" : "" }}</td>
          <td>{{ contact.wonEmmy ? "🌟" : "" }}</td>
          <td>
            <button @click="restoreContact(contact.id)" class="bordered-button">
              Delete
            </button>
          </td>
        </tr>
      </tbody>
    </table>
    <p v-else>No more contacts</p>
  </main>
</template>

<style></style>
