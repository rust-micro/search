<template>
  <h3>How it works:</h3>
  Type your search term in the search bar and<br />
  click on the link for the site you want to use.
  <br />
  <br />
  <input @focus="disableHotkeys" @blur="enableHotkeys" ref="search" type="text" placeholder="Search"
    v-model="search_input" style="width:100%" />

  <br />
  <ul>
    <li v-for="(link, name) in websites">
      <a :href="parseUrl(link[1])" target="_blank"><span style="width: 100px;" width="100px">({{ name }})</span> {{
        link[0] }}</a>
    </li>
  </ul>
</template>

<script>
export default {
  data() {
    return {
      search_input: "",
      hotkeys_enabled: true,
      websites: {
        "d": ["docs.rs", "https://docs.rs/releases/search?query=%s"],
        "r": ["rustdoc", "https://doc.rust-lang.org/std/?search=%s"],
        "c": ["crates.io", "https://crates.io/search?q=%s"],
        "e": ["rust by example", "https://doc.rust-lang.org/rust-by-example/?search=%s"],
        "o": ["roogle", "https://roogle.hkmatsumoto.com/search?query=%s&scope=set%3Alibstd"],
        "c": ["error_codes", "https://doc.rust-lang.org/error_codes/%s.html"],
        "l": ["clippy lints", "https://rust-lang.github.io/rust-clippy/master/#%s"],
        "n": ["rustdoc (nightly)", "https://doc.rust-lang.org/nightly/reference/?search=%s"],
        "v": ["rust version", "https://github.com/rust-lang/rust/blob/master/RELEASES.md?version=%s"],
        "u": ["can I use?", "https://caniuse.rs/#q=%s"]
      }
    }
  },
  created() {
    window.addEventListener("keydown", this.keydown)
  },
  destroyed() {
    window.removeEventListener("keydown", this.keydown)
  },
  watch: {
    search_input: function (newVal, oldVal) {
      const params = new URLSearchParams(window.location.search);
      params.set("q", newVal);
      window.history.replaceState({}, '', `${location.pathname}?${params.toString()}`);
    }
  },
  mounted() {
    this.$refs.search.focus();

    const params = new URLSearchParams(window.location.search);
    if (params.has("q")) {
      this.search_input = params.get("q");
    }
  },
  methods: {
    keydown(event) {
      if (this.hotkeys_enabled && event.key in this.websites) {
        console.log(event.key)
        let found = this.websites[event.key];
        console.log(found);
        if (found !== undefined) {
          let url = this.parseUrl(found[1]);
          window.open(url, "_blank");
        }
      }
    },
    enableHotkeys() {
      this.hotkeys_enabled = true;
    },
    disableHotkeys() {
      this.hotkeys_enabled = false;
    },
    parseUrl(link) {
      return link.replace('%s', encodeURIComponent(this.search_input));
    }
  },
  name: "SearchBar"
}
</script>

<style scoped></style>