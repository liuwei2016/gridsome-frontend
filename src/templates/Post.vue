<template>
  <Layout>
    <!-- Page Header -->
    <header class="masthead" style="background-image: url('/img/post-bg.jpg')">
      <div class="overlay"></div>
      <div class="container">
        <div class="row">
          <div class="col-lg-8 col-md-10 mx-auto">
            <div class="post-heading">
              <h1>{{ $page.post.title }}</h1>
              <!-- <h2 class="subheading">Problems look mighty small from 150 miles up</h2> -->
              <span class="meta">Posted by {{ $page.post.created_at }}</span>
            </div>
          </div>
        </div>
      </div>
    </header>
    <!-- Post Content -->
    <article>
      <div class="container">
        <div class="row">
          <div
            class="col-lg-8 col-md-10 mx-auto"
            v-html="mdToHtml($page.post.content)"
          ></div>
        </div>
      </div>
    </article>
  </Layout>
</template>
<page-query>
query ($id:ID!) {
    post: strapiPost (id:$id){
        id
        title
        content
        tags {
            id
            title
        }
    }
}
</page-query>
<script>
import MarkdownIt from "markdown-it";
const md = new MarkdownIt({
        highlight: function (str, lang) {
          // 添加这两行才能正确显示 <>
          str = str.replace(/&lt;/g, "<");
          str = str.replace(/&gt;/g, ">");

          if (lang && hljs.getLanguage(lang)) {
            try {
              return (
                '<pre class="hljs"><code>' +
                hljs.highlight(lang, str, true).value +
                "</code></pre>"
              );
            } catch (__) {}
          }

          return (
            '<pre class="hljs"><code>' +
            md.utils.escapeHtml(str) +
            "</code></pre>"
          );
        },
      });
var hljs = require("highlight.js");
export default {
  name: "PostPage",
  methods: {
    mdToHtml(markdown) {
      return md.render(markdown)
    },
  },
};
</script>
<style></style>