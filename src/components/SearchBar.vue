<template>
  <h3>How it works:</h3>
  Type your search term in the search bar and<br />
  click on the link for the site you want to use.
  <br />
  <br />
  <input @focus="disableHotkeys" @blur="enableHotkeys" ref="search" type="text" placeholder="Search"
    v-model="search_input" style="width:100%" />

  <br />
  <table>
    <tr v-for="(link, name) in websites">
      <td style="display: list-item; margin-left: 1em;">
        ({{ name }})
      </td>
      <td>
        <a href="">
          {{ link[0] }}
        </a>
      </td>
    </tr>
  </table>
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
    open(link) {
      window.open(link, "_blank");
    },
    keydown(event) {
      if (this.hotkeys_enabled) {
        if (event.key in this.websites) {
          let found = this.websites[event.key];
          if (found !== undefined) {
            let url = this.parseUrl(found[1]);
            this.open(url);
          }
        } else if (event.key === "s") {
          let temp = this.search_input;
          this.$refs.search.focus();
          this.search_input = temp;
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