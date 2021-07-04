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
            Date
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
        <tr class="text-gray-700" v-for="(item, key) in country" :key="key">
          <td class="border-b-2 p-4 dark:border-dark-5">{{ key }}</td>
          <td class="border-b-2 p-4 dark:border-dark-5">
            {{ item.Date | formatDate }}
          </td>
          <td class="border-b-2 p-4 dark:border-dark-5">
            {{ item.Confirmed }}
          </td>
          <td class="border-b-2 p-4 dark:border-dark-5">
            {{ item.Deaths }}
          </td>
          <td class="border-b-2 p-4 dark:border-dark-5">
            {{ item.Recovered }}
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  props: ['country'],
  filters: {
    formatDate(date) {
      const currentDate = new Date(date)
      return Intl.DateTimeFormat(undefined, {
        year: 'numeric',
        month: '2-digit',
        day: '2-digit',
      }).format(currentDate)
    },
  },
}
</script>

<style></style>
