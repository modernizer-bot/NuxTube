<template>
  <v-card class="mb-6">
    <v-col cols="12" v-if="$fetchState.pending">
      <div class="text-center pa-1">
        <v-progress-circular
          indeterminate
          color="primary"
        ></v-progress-circular>
      </div>
    </v-col>
    <div class="d-flex justify-space-between">
      <div>
        <v-card-title v-if="results.title">
          {{ results.title }}
        </v-card-title>
        <v-card-subtitle v-if="results.estimatedItemCount">
          {{ results.estimatedItemCount }} videos -
          {{ formatViews(results.views) }} views - {{ results.lastUpdated }}
        </v-card-subtitle>
      </div>
      <div class="d-flex align-end pa-2">
        <v-btn icon large>
          <v-icon >mdi-playlist-plus</v-icon>
        </v-btn>
      </div>
    </div>

    <v-divider></v-divider>
    <v-list max-height="400" style="overflow-y: auto">
      <v-list-item-group v-model="selected">
        <template v-for="(item, index) in results.items">
          <v-list-item :key="item.id" @click="itemClick(item.id, index)">
            <v-list-item-icon>
              <v-img width="96" height="56" :src="item.bestThumbnail.url">
                <template v-slot:placeholder>
                  <v-row class="fill-height">
                    <v-col cols="12">
                      <v-skeleton-loader type="image"></v-skeleton-loader>
                    </v-col>
                  </v-row>
                </template>
              </v-img>
            </v-list-item-icon>
            <v-list-item-content>
              <v-list-item-title v-html="item.title"></v-list-item-title>
              <v-list-item-subtitle
                v-html="item.author.name"
              ></v-list-item-subtitle>
            </v-list-item-content>
            <v-list-item-action>
              <v-list-item-action-text
                v-text="index + 1"
              ></v-list-item-action-text>
              <v-list-item-action-text
                v-text="item.duration"
              ></v-list-item-action-text>
            </v-list-item-action>
          </v-list-item>
          <v-divider :key="index"></v-divider>
        </template>
      </v-list-item-group>
    </v-list>
  </v-card>
</template>
<script>
import utils from "../../utils/utils";
export default {
  props: {
    playlistId: "",
    index: 0,
  },
  data() {
    return {
      results: [],
      selected: 0,
    };
  },
  watch: {
    index(val) {
      this.selected = val;
    },
  },
  fetchOnServer: false,
  methods: {
    formatViews(viewCount) {
      return utils.formatNumbers(viewCount);
    },
    itemClick(id, index) {
      this.$router.push({
        name: "player",
        query: {
          id: id,
          playlistId: this.playlistId,
          index: index,
        },
      });
    },
  },
  mounted() {
    this.selected = Number(this.index);
  },
  computed: {},
  async fetch() {
    this.results = await this.$axios.$get("/api/playlist", {
      params: {
        playlistId: this.playlistId,
      },
    });
  },
};
</script>