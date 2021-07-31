<template>
<Layout>
    <!-- Page Header -->
  <header 
    class="masthead" 
    
    :style="{backgroundImage: `url(${siteImage})`}"
  >
    <div class="overlay"></div>
    <div class="container">
      <div class="row">
        <div class="col-lg-8 col-md-10 mx-auto">
          <div class="site-heading">
            <h1>{{$page.general.edges[0].node.title}}</h1>
            <span class="subheading">{{$page.general.edges[0].node.subtitle}}</span>
          </div>
        </div>
      </div>
    </div>
  </header>

  <!-- Main Content -->
  <div class="container">
    <div class="row">
      <div class="col-lg-8 col-md-10 mx-auto">
        <div class="post-preview" v-for="edge in $page.posts.edges" :key="edge.node.id">
          <!-- <a href="post.html">
            <h2 class="post-title">
              {{edge.node.title}}
            </h2>
            <h3 class="post-subtitle">
              {{edge.node.content}}
            </h3>
          </a>-->
          <g-link :to="'/post/'+edge.node.id">
              <h2 class="post-title">{{edge.node.title}}</h2>
          </g-link>
          <p class="post-meta">Posted by
            <g-link to="/">dawei</g-link>
            {{ dayjsFormat(edge.node.created_at) }}
          </p>
          <p>
            <span v-for="tag in edge.node.tags" :key="tag.id">
              <g-link :to="'/tag/'+tag.id">{{ tag.title }}</g-link>
               &nbsp; &nbsp;
            </span>
          </p>
        </div>
        <hr>
        <!-- Pager -->
        <!-- <div class="clearfix">
          <a class="btn btn-primary float-right" href="#">Older Posts &rarr;</a>
        </div> -->
        <pager class="page-nav" :info="$page.posts.pageInfo" />
      </div>
    </div>
  </div>
</Layout>
</template>
<page-query>
query ($page: Int) {
    posts: allStrapiPost(perPage:5, page:$page) @paginate {
        pageInfo {
          totalPages
          currentPage
        }
        edges {
            node {
                id
                title
                content
                cover {
                  url
                }
                tags {
                    id
                    title
                }
                created_at
            }
        }
    }

    general: allStrapiGeneral {
      edges {
        node {
          id
          title
          subtitle
          cover {
            url,
            name
          }
        }
      }
    }
}

</page-query>

<script>
import { Pager } from 'gridsome'
import dayjs from 'dayjs'
 

export default {
  name:'HomePage',
  metaInfo: {
    title: '大伟的博客'
  },
  components: {
    Pager
  },

  data:function(){
    return{
       siteImage : 'banner/'+ Math.ceil(Math.random()*9) +".jpg"
    }
  },
  methods:{
    dayjsFormat: function(val) {
         return dayjs(val).format('YYYY-MM-DD hh:mm:ss')
    }
  }
}
</script>

<style>
.home-links a {
  margin-right: 1rem;
}
.page-nav a{  display: inline-block; margin-right: 4px;}
</style>
