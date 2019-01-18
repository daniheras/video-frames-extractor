<template>
    <div class="container">
        <!-- <video v-if="!!currentVideo" :src="videoUrl" autoplay></video> -->
        <!-- <d-player v-if="!!currentVideo" :src="videoUrl" autoplay></d-player> -->
        <!-- <img :src="currentSrc" alt=""> -->
        <b-modal hide-footer hide-header-close id="snapshotsettings" title="Snapshot Settings">
            <b-form>
                <b-form-group 
                    v-if="automaticallySave"
                    id="exampleInputGroup1"
                    label="Snapshots destination folder:"
                    label-for="exampleInput1"
                    description="Enter the path where your snapshots will be extracted">
                    <b-form-input id="exampleInput1"
                                type="email"
                                required
                                placeholder="Enter a valid path"
                                v-model="path"
                                >
                    </b-form-input>
                </b-form-group>
                <b-form-group id="exampleGroup4">
                    <b-form-checkbox-group v-model="automaticallySave" id="exampleChecks">
                    <b-form-checkbox value="automaticallySave">Save screenshots automatically</b-form-checkbox>
                    </b-form-checkbox-group>
                </b-form-group>
            </b-form>
        </b-modal>
        <div class="videoUrl">
            <b-form-file @input="videoUpload" accept="video/*" v-model="files" placeholder="Upload your video..."></b-form-file>
        </div>
        <div class="videoPlayer">
            <div class="videoWrapper">
                <d-player @play="playVideo" ref="player" :options="options"></d-player>
            </div>
        </div>
        <div class="appControls">
            <img v-b-modal.snapshotsettings class="camera-button" src="@/assets/setting.svg" alt="Camera Button">
            <div class="btn btn-primary" @click="takeScreenshot">
                Take Screenshot
            </div>
        </div>
        <!-- <div class="loadVideo">
            <input class="btn btn-primary" type="file">
        </div> -->
    </div>
</template>
<script>
import VideoSnapshot from 'video-snapshot';
import path from 'path'
import { remote } from 'electron'
import fs from 'fs';
import VueDPlayer from 'vue-dplayer'
import 'vue-dplayer/dist/vue-dplayer.css'
import Base64Binary from '../../utils/base64binary.js'

export default {
    components: {
        'd-player': VueDPlayer
    },
    mounted(){
        this.player = this.$refs.player.dp;
    },
    data: function(){
        return {
            automaticallySave: 'automaticallySave',
            path: `${require('os').homedir()}/Desktop/screenshots/`,
            secondsBeetweenFrames: 1,
            files: [],
            currentSrc: '',
            currentVideo: '',
            player: null,
            options: {
                screenshot: true,
                loop: true,
                volume: 0,
                mutex: true,
                video: {
                    url: 'http://clips.vorwaerts-gmbh.de/big_buck_bunny.mp4',
                    pic: ''
                }
            }
        }
    },
    methods: {
        processVideos(){
            this.currentVideo = this.files[0].path;
            //this.currentVideo = 'file://'+this.files[0].path;
            // const snapshoter = new VideoSnapshot(this.files[0]);
            // const url = snapshoter.takeSnapshot(10);
            // url.then(
            //     res => {
            //         this.currentSrc = res;
            //     }
            // )
            //console.log( url )
        },
        videoUpload(){
            this.currentVideo = 'file://'+this.files.path;
            this.player.switchVideo({
                url: 'file://'+this.files.path
            })
        },
        playVideo(){
            // console.log(this.currentVideo)
            // console.log(this.options.video.url)
        },
        takeScreenshot(){
            //console.log(new File(this.player.video.src),URL.createObjectURL(object))
            const snapshoter = new VideoSnapshot(this.files);
            const url = snapshoter.takeSnapshot(this.player.video.currentTime);
            url.then(
                res => {
                    // First we create the path if it doesnt exists
                    fs.mkdir(this.path, { recursive: true }, (err) => {
                        if (err) throw err;
                    });
                    // Then we write the image file inside that folder
                    fs.writeFile(this.path+`/screenshot${new Date().getTime()}.png`, Base64Binary.decode(res.split(',')[1]), 'utf8', 
                        err => {
                            if ( err ) console.log(err);
                            console.log('File saved')
                        }
                    )
                }
            )
        },
    }
}
</script>
<style lang="scss">
    
    .container{
        height: 100%;
        flex-grow: 1;
        display: flex;
        flex-direction: column;
        padding-bottom: 3rem;
    }
    
    .videoPlayer{
        display: flex;
        justify-content: center;
        padding: 2rem;
        border: 5px solid #fff;
        background: #fff;
    }

    .videoWrapper{
        width: 80vh;
    }

    .appControls{
        background: #fff;
        padding: 1rem;

        .camera-button{
            display: inline-block;
            width: 50px;
            cursor: pointer;
            transition: .3s;
            margin-right: 1rem;

            &:hover{
                transform: rotate(15deg);
            }
        }
    }
</style>
