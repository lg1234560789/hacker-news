<template>
     <li class="new-item">
        <div class="new-title">
            <span class="indexNum">{{ index }}.</span>
            <span :class="{hide: item.voted}" class="vote" @click="voted(item)"></span>
            <span class="title">
                <a :href="item.url" class="url" target="_black">{{ item.title }}</a>
                <span>(<a href="#">{{ item.type }}</a>)</span>
            </span>
        </div>
        <span class="meta">
            <span>{{ item.score }}</span> points by
            <span><router-link :to="'/user/' + item.by">{{ item.by }}</router-link></span>
            <span class="timeago">
                <router-link :to="'/item/' + item.id">{{ item.time | timeAgo }}</router-link>
            </span>
            <span v-show="item.voted" @click="item.voted=!item.voted"><a href="#">unvote</a></span>
            <span @click="hideItem(item)"><a href="#">hide</a></span>
            <span><router-link :to="'/item/' + item.id">{{ item.kids.length }} comments</router-link></span>
        </span>
    </li>
</template>

<script>
    export default {
      name: 'item',
      props: ['item', 'index'],
      methods: {
        voted (item) {
          this.$store.commit('UPDATE_VOTED', item.id)
        },
        hideItem (item) {
          this.$store.commit('HIDE_ITEM', item)
        }
      }
    }
</script>

<style lang="scss">
    .new-item {
        width: 100%;
        margin-bottom: 0.5em;
        color: #9a9a9a;
        background: #f6f6ef;
        padding: 10px;

        a {
            color: #9a9a9a;

            &:hover {
                text-decoration: underline;
            }
        }
    }
    .new-title {
        display: flex;
        margin: 0 0 0.3em 0;
        .vote {
            display: block;
            width: 0;
            height: 0;
            overflow: hidden;
            border: 0.45em solid transparent;
            border-bottom-color: #9a9a9a;
            margin: 0 0.3em;
            cursor: pointer;
        }

        .hide {
            visibility: hidden;
        }


        .url {
            color:black;
            font-size: 1em;
        }
    }

    .meta {
        padding: 0 0 0 1.6em;
        display: flex;

        span {
            display: block;
            padding: 0 2px;
        }
        span:nth-child(4) {
            margin: 0 0 0 2px;
        }
        span:nth-child(4), span:nth-child(5), span:nth-child(6) {
            padding: 0 6px;
            display: block;
            border-left: 1px solid #9a9a9a;
        }
    }

</style>
