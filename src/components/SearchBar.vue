<script setup>
import Caniusers from "@/components/Caniusers.vue";
</script>

<template>
  <input type="text" placeholder="Search" v-model="search_input"/>
  <button type="submit">Submit</button>

  <br/>
  <ul>
    <li v-for="(link, name) in websites">
      <a :href="link.replace('%s', encodeURIComponent(search_input))" target="_blank">{{ name }}</a>
    </li>
    <li>
      <a href="https://caniuse.rs">canisuse.rs</a>
    </li>
  </ul>
  <br />
  <Caniusers :search_input="search_input" />
</template>

<script>
export default {
  data() {
    return {
      search_input: "",
      websites: {
        "docs.rs": "https://docs.rs/releases/search?query=%s",
        "rustdoc": "https://doc.rust-lang.org/std/?search=%s",
        "crates.io": "https://crates.io/search?q=%s",
        "rust by example": "https://doc.rust-lang.org/rust-by-example/?search=%s",
        "roogle": "https://roogle.hkmatsumoto.com/search?query=%s&scope=set%3Alibstd",
        "error_codes": "https://doc.rust-lang.org/error_codes/%s.html",
        "clippy lints": "https://rust-lang.github.io/rust-clippy/master/#%s",
        "rustdoc (nightly)": "https://doc.rust-lang.org/nightly/reference/?search=%s",
        "rust version": "https://github.com/rust-lang/rust/blob/master/RELEASES.md?version=%s"

      }
    }
  },
  name: "SearchBar"
}
</script>

<style scoped>

</style>