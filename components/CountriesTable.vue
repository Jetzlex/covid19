<template>
  <div>
    <table class="table w-full p-4 bg-white shadow rounded-lg">
      <thead>
        <tr>
          <th
            class="
              border-b-2
              font-semibold
              text-left
              p-4
              dark:border-dark-5
              whitespace-nowrap
              font-normal
              text-gray-900
            "
          >
            #
          </th>
          <th
            class="
              border-b-2
              font-semibold
              text-left
              p-4
              dark:border-dark-5
              whitespace-nowrap
              font-normal
              text-gray-900
            "
          >
            Country
          </th>
          <th
            class="
              border-b-2
              font-semibold
              text-left
              p-4
              dark:border-dark-5
              whitespace-nowrap
              font-normal
              text-gray-900
            "
          >
            Confirmed
          </th>
          <th
            class="
              border-b-2
              font-semibold
              text-left
              p-4
              dark:border-dark-5
              whitespace-nowrap
              font-normal
              text-gray-900
            "
          >
            Deaths
          </th>
          <th
            class="
              border-b-2
              font-semibold
              text-left
              p-4
              dark:border-dark-5
              whitespace-nowrap
              font-normal
              text-gray-900
            "
          >
            Recovered
          </th>
        </tr>
      </thead>
      <tbody>
        <tr
          class="text-gray-700"
          v-for="(getCountry, key) in allCountries"
          :key="key"
        >
          <td class="border-b-2 p-4 dark:border-dark-5">{{ key }}</td>
          <td class="border-b-2 p-4 dark:border-dark-5">
            {{ getCountry.Country }}
          </td>
          <td class="border-b-2 p-4 dark:border-dark-5">
            {{ getCountry.TotalConfirmed }}
          </td>
          <td class="border-b-2 p-4 dark:border-dark-5">
            {{ getCountry.TotalDeaths }}
          </td>
          <td class="border-b-2 p-4 dark:border-dark-5">
            {{ getCountry.TotalRecovered }}
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
    props: ["allCountries"]
}
</script>

<style></style>
