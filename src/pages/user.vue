<template>
  <b-container fluid>
    <b-card-group columns>
      <b-card
        v-for="edge in user.edge_owner_to_timeline_media.edges"
        :key="edge.node.id"
        no-body
        img-top
        :img-src="edge.node.display_url"
        :img-alt="edge.node.accessibility_caption"
      >
        <b-card-body>
          <b-card-text>
            {{ edge.node.edge_media_to_caption.edges[0].node.text }}
          </b-card-text>
        </b-card-body>
        <b-card-footer>
          <b-icon icon="heart-fill" variant="danger"></b-icon>
          <numeral :number="edge.node.edge_liked_by.count" /> likes
        </b-card-footer>
      </b-card>
    </b-card-group>
  </b-container>
</template>

<script lang="ts">
import Vue from "vue";
import { UserDataI } from "../types";
import Numeral from "~/components/numeral";

export default Vue.extend({
  async asyncData({ query, $axios }) {
    const { username } = query;

    const { data }: { data: UserDataI } = await $axios.get(
      `https://www.instagram.com/${username}/?__a=1`
    );

    const { user } = data.graphql;

    return { user };
  },
  components: {
    Numeral
  }
});
</script>
