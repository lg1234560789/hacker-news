<template>
  <div id="app">
    <div class="slideBar">
      <header class="logo">
        <img src="https://news.ycombinator.com/favicon.ico" alt="Hacker News">
        <h1 @click="cancleChecked()"><router-link to="/news">Hacker News</router-link></h1>
      </header>
      <nav class="nav">
          <ul>
              <li v-for="menu in menus" v-show="menu.isShow" :class="{ active: menu.checked }" @click="updateMenu(menu)">
                <router-link :to="menu.to"><i :class="menu.icon"></i>{{ menu.name }}</router-link>
              </li>         
          </ul>
      </nav>
      <div class="users">
        <a href="#"><i class="iconfont icon-users"></i></a>
      </div>
    </div>
    <div class="main">
      <header class="top">
        <h2>{{ title }}</h2>
        <div class="search">
          <i class="iconfont icon-search"></i>
          <input type="search" placeholder="search">
        </div>
        <span class="userName" v-show="logged">{{ user.id }}</span>
        <span class="login" v-show="!logged"><router-link to="/login">login</router-link></span>
        <span class="login" v-show="logged"><a href="#" @click="logout">logout</a></span>
      </header>
      <section class="content">
        <div class="list">
          <keep-alive>
            <router-view></router-view>
          </keep-alive>
        </div>
      </section>
    </div>
  </div>
</template>

<script>
import { mapState } from 'vuex'
export default {
  name: 'app',
  created () {
    this.$store.commit('GET_USERINFO')
  },
  computed: {
    ...mapState({
      menus: 'menus',
      title: 'title',
      logged: state => state.loginUser.logged
    }),
    user () {
      return this.$store.state.loginUser
    }
  },
  methods: {
    updateMenu (menu) {
      this.$store.commit('UPDATE_MENU', menu.id)
    },

    cancleChecked () {
      this.$store.commit('CANCLE_CHECKED')
    },
    logout () {
      const user = this.$store.state.loginUser
      user.logged = false
      //  将改变状态的用户重新设置
      localStorage.setItem('user', JSON.stringify(user))
    }
  }
}
</script>

<style lang="scss">

#app {
  width: 86vw;
  height: 90vh;
  font-size: 1rem;
  display: flex;
}

.slideBar {
    width: 20%;
    height: 100%;
    background:white;
    display: flex;
    flex-wrap: wrap;
    border-bottom-left-radius: 6px;
    border-top-left-radius: 6px;
    box-shadow: 0 1px 1px rgba(0,0,0,0.2);
}

header.logo {
  width: 100%;
  height: 18%;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size:1rem;

  img {
    width: 2.5em;
    height: 2.5em;
    border-radius: 50%;
  }

  h1 {
    width: 65%;
    font-size: 1.25em;
    text-align: center;
    
  }

  h1 a {
    color: black;
  }
}

nav.nav {
  width:100%;
  height:70%;

  ul {
    display:flex;
    flex-wrap: wrap;
    font-size:0.80em;
  }

  ul li {
    width:100%;
    display:flex;
    justify-content:flex-start;
    align-items:center;
    transition:all 0.2s;
  }

  ul li i {
    color:#bdbdbd;
    padding:1rem 1.5rem;
    transition:all 0.2s;
    cursor: pointer;
  }

  ul li a {
    display:block;
    color:#bdbdbd;
    padding:1rem 0;
    letter-spacing:1px;
    transition:all 0.2s;

    &:hover {
      color:#252525;
      font-weight:bold;
    }
  }

  ul li.active {
    border-left:3px solid #ff6600;
  }

  ul li.active a, ul li.active i{
    color:#000000;
    font-weight:bold;
  }

}

.users {
  height:12%;
  width:100%;
  display:flex;
  justify-content:center;
  align-items:center;

  a {
    display:block;
    width:80%;
    height:50%;
    background:#f8f8f8;
    display:flex;
    justify-content:center;
    align-items:center;
  }
}

div.main {
  width: 80%;
  height: 100%;
  background: #f7f7f7;
  border-bottom-right-radius: 6px;
  border-top-right-radius: 6px;
  box-shadow: 0 1px 1px rgba(0,0,0,0.2);
  display: flex;
  flex-wrap: wrap;
  font-size: 1rem;
  overflow: hidden;

  @at-root {
    .top{
      height: 20%;
      width: 100%;
      display: flex;
      justify-content: center;
      align-items: center;

      h2 {
        flex: 1 1 20%;
        font-size: 1em;
        text-align: center;
      }

      .search {
        flex: 1 1 55%;
        display: flex;
        justify-content: flex-end;
        position: relative;

        i {
          position: absolute;
          top: 30%;
          left: 52%;
          color: #bdbdbd;
        }

        input[type="search"] {
          width: 50%;
          padding: 0.6em 2.1em;
          color: #bdbdbd;
          border: 0;
          font-size: 1rem;
          outline: none;
          border-radius: 2em;
          box-shadow: 1px 0 5px rgba(0, 0, 0, 0.1);
        }

      }

      .userName, .login {
        margin-left: 1em;
        display: block;
        flex: 1 1 10%;
        text-align: center;
      }

      .login {
        text-align: left;
        border-left: 1px solid black;
        padding-left: 0.5em;

      }


    }
  
    .content {
      width: 100%;
      height: 100%;
      
      display: flex;
      justify-content: center;
      align-items: center;

      .list {
        height: 100%;
        width: 92%;
        background: white;
        padding: 1em 2em 8em 2em;
        overflow-y: scroll;
      }
    }
  
  }
  
}



</style>
