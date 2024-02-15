<script setup>
import { RouterView } from 'vue-router'
import TheHeader from './components/TheHeader.vue'

// import { onMounted } from 'vue'

// onMounted(() => {
//   let PROJECT_ID = 'c7jd3ho8'
//   let DATASET = 'production'
//   let QUERY = encodeURIComponent('*[_type == "about"]')

//   // Compose the URL for your project's endpoint and add the query
//   let URL = `https://${PROJECT_ID}.api.sanity.io/v2021-10-21/data/query/${DATASET}?query=${QUERY}`

//   fetch(URL)
//     .then((res) => res.json())
//     .then(({ result }) => {
//       console.log(result)
//       // get the list element, and the first item
//       let list = document.querySelector('ul')
//       let firstListItem = document.querySelector('ul li')

//       if (result.length > 0) {
//         // remove the placeholder content
//         list.removeChild(firstListItem)

//         result.forEach((pet) => {
//           // create a list element for each pet
//           let listItem = document.createElement('li')

//           // add the pet name as the text content
//           listItem.textContent = pet?.name

//           // add the item to the list
//           list.appendChild(listItem)
//         })
//         let pre = document.querySelector('pre')
//         // add the raw data to the preformatted element
//         pre.textContent = JSON.stringify(result, null, 2)
//       }
//     })
//     .catch((err) => console.error(err))
// })
</script>

<template>
  <header>
    <div class="wrapper">
      <nav>
        <TheHeader />
      </nav>
    </div>
  </header>

  <RouterView />
</template>

<style scoped>
.logo {
  display: block;
  margin: 0 auto 2rem;
}

nav {
  width: 100%;
  margin-bottom: 2rem;
  padding: 1rem 3rem 0 3rem;
}

@media (max-width: 769px) {
  nav {
    padding: 1rem 0 0 0;
  }
}
</style>
