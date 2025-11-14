<script setup>
import { ref } from 'vue'
import Tabs from './Tabs.vue'
import Tab from './Tab.vue'

const current1 = ref('profile')
const current2 = ref('dashboard')
const current3 = ref('home')
</script>

<template>
  <div class="space-y-8 p-8">
    <h1 class="text-3xl font-bold text-center">Tabs Demo</h1>

    <!-- Underline Variant -->
    <section class="space-y-3">
      <h2 class="text-xl font-semibold">Underline Variant</h2>
      <Tabs v-model="current1" variant="underline">
        <Tab title="Profile" slug="profile">
          <div class="p-6 bg-white rounded-lg shadow dark:bg-gray-800">
            <h3 class="text-lg font-semibold mb-2">User Profile</h3>
            <p class="text-gray-600 dark:text-gray-400">
              View and edit your personal information including avatar, name, email, and contact details.
            </p>
          </div>
        </Tab>
        <Tab title="Settings" slug="settings">
          <div class="p-6 bg-white rounded-lg shadow dark:bg-gray-800">
            <h3 class="text-lg font-semibold mb-2">Account Settings</h3>
            <p class="text-gray-600 dark:text-gray-400">
              Change your password, update preferences, and configure your account settings.
            </p>
          </div>
        </Tab>
        <Tab title="Contacts" slug="contacts">
          <div class="p-6 bg-white rounded-lg shadow dark:bg-gray-800">
            <h3 class="text-lg font-semibold mb-2">Contact Information</h3>
            <p class="text-gray-600 dark:text-gray-400">
              Manage your addresses, phone numbers, and other important contact information.
            </p>
          </div>
        </Tab>
      </Tabs>
    </section>

    <!-- Pills Variant -->
    <section class="space-y-3">
      <h2 class="text-xl font-semibold">Pills Variant</h2>
      <Tabs v-model="current2" variant="pills">
        <Tab title="Dashboard" slug="dashboard">
          <div class="p-6 bg-white rounded-lg shadow dark:bg-gray-800">
            <h3 class="text-lg font-semibold mb-2">Dashboard Overview</h3>
            <p class="text-gray-600 dark:text-gray-400">
              Quick summary of your account activity, stats, and notifications.
            </p>
          </div>
        </Tab>
        <Tab title="Projects" slug="projects">
          <div class="p-6 bg-white rounded-lg shadow dark:bg-gray-800">
            <h3 class="text-lg font-semibold mb-2">Projects</h3>
            <p class="text-gray-600 dark:text-gray-400">
              View all your projects, their status, and upcoming deadlines.
            </p>
          </div>
        </Tab>
        <Tab title="Reports" slug="reports">
          <div class="p-6 bg-white rounded-lg shadow dark:bg-gray-800">
            <h3 class="text-lg font-semibold mb-2">Reports</h3>
            <p class="text-gray-600 dark:text-gray-400">
              Generate and review detailed reports and analytics for your projects.
            </p>
          </div>
        </Tab>
        <Tab title="Team" slug="team">
          <div class="p-6 bg-white rounded-lg shadow dark:bg-gray-800">
            <h3 class="text-lg font-semibold mb-2">Team Members</h3>
            <p class="text-gray-600 dark:text-gray-400">
              Check your team’s members, roles, and contact information.
            </p>
          </div>
        </Tab>
      </Tabs>
    </section>

    <!-- Boxed Variant -->
    <section class="space-y-3">
      <h2 class="text-xl font-semibold">Boxed Variant</h2>
      <Tabs v-model="current3" variant="boxed">
        <Tab title="Home" slug="home">
          <div class="p-6 bg-white rounded-lg shadow dark:bg-gray-800">
            <h3 class="text-lg font-semibold mb-2">Home Page</h3>
            <p class="text-gray-600 dark:text-gray-400">
              Welcome to the homepage! Find the latest updates and featured content here.
            </p>
          </div>
        </Tab>
        <Tab title="About" slug="about">
          <div class="p-6 bg-white rounded-lg shadow dark:bg-gray-800">
            <h3 class="text-lg font-semibold mb-2">About Us</h3>
            <p class="text-gray-600 dark:text-gray-400">
              Learn about our company, mission, and core values.
            </p>
          </div>
        </Tab>
        <Tab title="Services" slug="services">
          <div class="p-6 bg-white rounded-lg shadow dark:bg-gray-800">
            <h3 class="text-lg font-semibold mb-2">Our Services</h3>
            <p class="text-gray-600 dark:text-gray-400">
              Explore the range of services we provide to help you achieve your goals.
            </p>
          </div>
        </Tab>
      </Tabs>
    </section>
  </div>
</template>
