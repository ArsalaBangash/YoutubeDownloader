<template>
  <v-container>
    <div class="title mb-2">Welcome to the Youtube Downloader!</div>
    <v-text-field v-model="url" label="Solo" placeholder="Enter URL" solo></v-text-field>
    <v-select :items="qualities" label="Select a quality" solo></v-select>
    <v-btn @click="downloadYoutubeVideo" primary>Download</v-btn>
    <div class="body-1">Video Size: {{size}}</div>
  </v-container>
</template>

<script>
const fs = require("fs");
const youtubedl = require("youtube-dl");

const downloadVideo = url => {
  const video = youtubedl(
    url,
    // Optional arguments passed to youtube-dl.
    ["--format=18"],
    // Additional options can be given for calling `child_process.execFile()`.
    { cwd: __dirname }
  );
  return video;
};

export default {
  data() {
    return {
      url: "",
      qualities: ["240p", "360p", "480p", "720p", "1080p"],
      downloadStarted: false,
      size: 0
    };
  },
  methods: {
    downloadYoutubeVideo() {
      const video = downloadVideo(this.url);
      // Will be called when the download starts.
      video.on("info", function(info) {
        this.downloadStarted = true
        this.size = info.size
      });

      video.pipe(fs.createWriteStream("myvideo.mp4"));
    }
  }
};
</script>
