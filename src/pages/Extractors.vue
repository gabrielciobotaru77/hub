<template>
  <Layout>
    <div class="plugins-overview w-full max-w-7xl mx-auto">
      <p class="text-3xl md:text-5xl pb-4 pt-8 font-bold font-pjs text-purple">Extractors</p>
      <div class="max-w-3xl mx-auto">
        <p class="mb-4">
          Meltano lets you easily extract data out of arbitrary sources (databases, SaaS APIs, and
          file formats) using Singer taps, which take the role of your project’s extractor plugins.
        </p>
        <p>
          To learn more about extracting and loading data using Meltano, refer to the
          <a href="https://docs.meltano.com/getting-started/part1" target="_blank" class="underline"
            >Getting Started guide</a
          >.
        </p>
      </div>
      <div
        class="grid grid-cols-2 md:grid-cols-4 rounded-lg p-4 mt-4 md:m-4 gap-4 w-full place-items-stretch"
        role="list"
      >
        <div
          v-for="edge in $page.allPlugins.edges"
          :key="edge.node.id"
          class="rounded-md text-slate-800 h-48 hover:bg-white bg-white-07 p-2 md:p-4 overflow-hidden border border-purple/10"
        >
          <g-link
            class="grid grid-rows-6 align-self-center place-items-center h-40 w-full"
            :to="edge.node.path.split('--')[0]"
          >
            <div class="grid place-self-center row-span-4 h-24 w-full bg-white">
              <g-image
                v-if="edge.node.logo_url"
                :src="
                  require(`!!assets-loader?width=175&height=80&fit=inside!@logos/${edge.node.logo_url.replace(
                    '/assets/logos/',
                    ''
                  )}`)
                "
                class="place-self-center"
              />
            </div>
            <div class="grid content-end row-span-5 self-stretch">
              <p class="font-bold underline text-xs lg:text-md justify-self-center text-clip">
                {{ edge.node.label }}
              </p>
            </div>
          </g-link>
        </div>
        <Pager
          :info="$page.allPlugins.pageInfo"
          class="pager-container text-black m-4 col-span-2 md:col-span-4 mx-auto"
          linkClass="pager-container__link px-2.5 py-1 hover:text-sky-300"
          activeLinkClass="bg-blue text-white rounded-full"
        />
      </div>
    </div>
  </Layout>
</template>

<script>
import { Pager } from "gridsome";

export default {
  name: "ExtractorsPage",
  components: {
    Pager,
  },
};
</script>

<page-query lang="graphql">
query ($page: Int) {
  allPlugins(
    perPage: 100
    page: $page
    sortBy: "label_lowercase"
    order: ASC
    filter: { isDefault: { eq: true }, pluginType: { eq: "extractor" }, hidden: { ne: true } }
  ) @paginate {
    pageInfo {
      totalPages
      currentPage
    }
    edges {
      node {
        id
        description
        path
        label
        name
        pluginType
        logo_url
        namespace
        variant
        pip_url
        repo
        maintenance_status
        keywords
      }
    }
  }
}
</page-query>

<style lang="scss"></style>
