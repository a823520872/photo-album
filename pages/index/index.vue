<template>
    <view class="m_content">
        <view class="album_list m_flex_wrap">
            <view class="item" v-for="(li, i) in album" :key="i" @tap="toPhoto(li)">
                <view class="box">
                    <image :src="li.photos[0]" mode="aspectFill"></image>
                </view>
                <view class="title">{{li.name}}</view>
            </view>
            <view class="item last" @tap="toCreate">
                <view class="box"></view>
            </view>
        </view>
    </view>
</template>

<script>
export default {
    data() {
        return {
            album: []
        }
    },
    onShow() {
        this.getData()
    },
    methods: {
        getData() {
            this.$store.dispatch('getProfile').then(() => {
                this.getAlbum()
            })
        },
        getAlbum() {
            this.$ajax('/api/album').then(res => {
                this.album = res.data || []
            })
        },
        toPhoto(li) {
            this.$goPage(`/pages/index/photo?id=${li.id}`)
        },
        toCreate() {
            this.$goPage('/pages/index/album')
        }
    }
}
</script>

<style lang="scss" scoped>
.album_list {
    padding: 24upx 20upx;

    .item {
        margin: 0 10upx 24upx;
    }

    .box {
        position: relative;
        width: 157upx;
        height: 157upx;
        border: 1upx solid $uni-border-color;
        border-radius: 12upx;
        overflow: hidden;
    }

    .title {
        width: 100%;
        padding: 10upx 0 20upx;
        line-height: 40upx;
        overflow: hidden;
        white-space: nowrap;
        text-overflow: ellipsis;
    }

    .last {
        .box {
            &::before,
            &::after {
                content: ' ';
                position: absolute;
                top: 50%;
                left: 50%;
                transform: translate(-50%, -50%);
                border-radius: 4upx;
            }

            &::before {
                width: 60%;
                height: 0;
                border-bottom: 8upx solid $uni-border-color;
            }

            &::after {
                width: 0;
                height: 60%;
                border-left: 8upx solid $uni-border-color;
            }
        }
    }
}
</style>
