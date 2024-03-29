<!--
Author: Yuki
Date: 2022-04-12 17:31:44
LastEditTime: 2022-04-21 17:23:03
Description: detail页面的基本框架
FilePath: \vite-music-player\src\components\TheFramePlaylist.vue
-->

<script lang="ts" setup>
import { mapActionsHelpers, mapMutationsHelpers } from '@/common/util'
import { reactive, watchEffect } from 'vue'
import { useStore } from '@/store'
import moment from 'moment'

const store = useStore()
const props = defineProps<{
  //创建时间
  createTime: string
  // 作者
  artist: string
  // 标题
  title: string
  // 副标题
  desc: string
  // 海报链接
  picUrl: string
  // 副标题
  sub?: string
  // 歌曲列表
  songs: Array<any>
}>()

const data = reactive({
  // 所有歌曲总时长，分钟
  duration: 0
})

const { getSongUrl } = mapActionsHelpers(null, ['getSongUrl'])
const { setAudioUrl, setAudioInfo, setLoading, setTips } = mapMutationsHelpers(
  null,
  ['setAudioUrl', 'setAudioInfo', 'setLoading', 'setTips']
)

setLoading(true)

watchEffect(() => {
  if (props.songs.length > 0) {
    // 歌单总时长
    let durationTotal: number = 0
    props.songs.forEach((song) => {
      durationTotal += moment.duration(song.duration, 'millisecond').minutes()
    })
    // 分钟数
    data.duration = durationTotal
    setLoading(false)
  }
})

/**
 * 单击li元素，播放音乐
 * @param songInfo
 */
function toPlaySong(songInfo: any) {
  setAudioInfo(songInfo)
  // // DOM路径
  // const bubblingPath: Array<HTMLElement> = e.path
  // // LI元素排在第几位
  // const liElementIdx: number = bubblingPath.map(el => el.toString()).indexOf('[object HTMLLIElement]')
  // // 找到歌曲id
  // const payload: string = e.path[liElementIdx].dataset['songId']

  // 获取歌曲url
  getSongUrl(songInfo.payload).then((url: string) => {
    if (url) setAudioUrl(url)
    else setTips('需要购买数字专辑')
  })
}
</script>

<template>
  <div id="detail-frame">
    <!--  歌单上半部 封面信息+按钮  -->
    <div class="detail-frame-poster">
      <!-- 封面 -->
      <div class="detail-frame-poster-img">
        <img :src="props.picUrl" alt="" />
      </div>
      <!-- 标题 -->
      <div>
        <h1 class="detail-frame-poster-desc-h">{{ props.title }}</h1>
      </div>
      <!-- 作者 -->
      <div>
        <p class="detail-frame-poster-desc-p">{{ props.artist }}</p>
      </div>
      <!-- 描述 -->
      <div>
        <span class="detail-frame-poster-desc-s">{{ props.desc }}</span>
      </div>
      <!-- 播放按钮 -->
      <div class="detail-frame-poster-action">
        <button class="detail-frame-action-btn">
          <span class="action-icon-play"></span>
          <span>播放</span>
        </button>
        <button class="detail-frame-action-btn">
          <span class="action-icon-random"></span>
          <span>随机播放</span>
        </button>
      </div>
    </div>

    <!--  歌单下半部 歌曲列表  -->
    <div class="detail-frame-list">
      <ul class="detail-frame-list-ul">
        <li
          v-for="(songItem, songIdx) in props.songs"
          :data-song-id="songItem.payload"
          :key="`song-${songIdx}`"
          @click="toPlaySong(songItem)"
          class="detail-frame-list-li"
          :class="{
            listening: songItem.payload === store.state.audioInfo.payload
          }"
        >
          <div class="list-index">
            <span class="list-index-text">{{ songIdx + 1 }}</span>
            <img class="list-index-icon" src="@/assets/equaliser.svg" alt="" />
          </div>
          <div class="list-desc">
            <div class="list-desc-title">
              {{ songItem.title }}
              <span class="fee-tips" v-if="songItem.fee === '4'"
                >(数字专辑)</span
              >
            </div>
            <div class="list-desc-sub">{{ songItem.artist }}</div>
          </div>
          <div class="list-select">...</div>
        </li>
      </ul>
    </div>

    <!-- 歌单歌曲总数，时长信息 -->
    <div class="detail-frame-info">
      <div>
        {{ props.createTime }}
      </div>
      <div>
        {{ props.songs.length }}&nbsp;首歌曲，{{ data.duration }}&nbsp;分钟
      </div>
    </div>
  </div>
</template>

<style lang="less">
#detail-frame {
  height: 100%;
  padding: 16px;
  position: relative;
  width: 100vw;
  overflow: hidden;

  // 上
  .detail-frame-poster {
    height: 50vh;
    display: grid;
    justify-content: center;
    grid-auto-columns: minmax(0, 1fr);
    grid-template-rows:
      [poster] minmax(180px, 2fr)
      [title] minmax(0, auto)
      [artist] minmax(0, auto)
      [desc] minmax(0, auto)
      [action] minmax(0, auto);

    .detail-frame-poster-img {
      height: 100%;
      width: auto;
      padding: 12px;
      display: flex;
      justify-content: center;
      font-size: 0;

      > img {
        display: block;
        height: 100%;
        width: auto;
        border-radius: 6px;
        box-shadow: 6px 6px 32px rgba(94, 84, 77, 0.6);
        object-fit: cover;
      }
    }

    .detail-frame-poster-desc-h {
      font-size: 19px;
      font-weight: 600;
      text-align: center;
      padding: 0 16px;
      display: -webkit-box;
      -webkit-box-orient: vertical;
      -webkit-line-clamp: 1;
      overflow: hidden;
    }

    .detail-frame-poster-desc-p {
      font-size: 18px;
      text-align: center;
      color: var(--theme-color);
    }

    .detail-frame-poster-desc-s {
      font-size: 11px;
      font-weight: 600;
      display: block;
      text-align: center;
      color: rgba(0, 0, 0, 0.45);
      display: -webkit-box;
      -webkit-box-orient: vertical;
      -webkit-line-clamp: 2;
      overflow: hidden;
    }

    .detail-frame-poster-action {
      margin-top: 10px;
      display: flex;
      justify-content: space-between;

      .detail-frame-action-btn {
        font-size: 15px;
        height: 48px;
        color: var(--theme-color);
        font-weight: 600;
        border: none;
        border-radius: 8px;
        width: calc(50% - 8px);
        background: rgba(0, 0, 0, 0.06);
        display: flex;
        align-items: center;
        justify-content: center;
        .action-icon-play {
          height: 11px;
          width: 11px;
          background: url('@/assets/icon-detail-play.png') center no-repeat;
          background-size: 100%;
          margin-right: 4px;
        }
        .action-icon-random {
          height: 16px;
          width: 16px;
          background: url('@/assets/icon-detail-random.png') center no-repeat;
          background-size: 100%;
          margin-right: 4px;
        }
      }
    }
  }

  //  下
  .detail-frame-list {
    width: 100%;

    .detail-frame-list-ul {
      width: 100%;
      position: relative;
      margin-top: 16px;
      border-top: 1px solid rgba(0, 0, 0, 0.1);

      .detail-frame-list-li {
        width: 100%;
        display: grid;
        grid-template-columns: [index] 32px [songInfo] 1fr [select] 32px;

        .list-index,
        .list-desc,
        .list-select {
          display: flex;
          align-items: center;
        }

        .list-index-icon {
          display: none;
        }

        .list-index,
        .list-select {
          font-weight: 600;
          font-size: 16px;
          color: rgba(0, 0, 0, 0.45);
          justify-content: center;
        }

        .list-desc {
          display: flex;
          flex-direction: column;
          align-items: flex-start;
          padding: 4px 32px 2px 10px;
          border-bottom: 1px solid rgba(0, 0, 0, 0.1);
          overflow: hidden;
          white-space: nowrap;
          text-overflow: ellipsis;

          .list-desc-title {
            font-size: 16px;

            .fee-tips {
              font-size: 16px;
              color: rgba(0, 0, 0, 0.45);
            }
          }

          .list-desc-sub {
            font-size: 12px;
            font-weight: 200;
            color: rgba(0, 0, 0, 0.45);
          }

          .list-desc-title,
          .list-desc-sub {
            width: 100%;
            display: block;
            overflow: hidden;
            white-space: nowrap;
            text-overflow: ellipsis;
          }
        }

        .list-select {
          border-bottom: 1px solid rgba(0, 0, 0, 0.1);
        }
      }

      .listening {
        .list-desc {
          .list-desc-title {
            color: var(--theme-color);
          }
        }
        .list-index {
          .list-index-text {
            display: none;
          }
          .list-index-icon {
            display: block;
          }
        }
      }
    }
  }

  // 歌单信息
  .detail-frame-info {
    margin: 16px 0;
    font-weight: 600;
    font-size: 12px;
    color: rgba(0, 0, 0, 0.45);
    > div {
      margin-top: 2px;
    }
  }
}
</style>

