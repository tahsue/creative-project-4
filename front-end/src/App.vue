<template>
<div id="app">
  <div class="header">
    <router-link to="/">
    </router-link>
    <div class="title">
      <h1>Hello World, Meet Someone New!</h1>
    </div>
    <div class="nav-bar">
      <router-link class="nav-button" to="/">Home</router-link>
      <router-link class="nav-button" to="/admin">Create/Edit</router-link>
      <router-link class="nav-button" to="/post">Post</router-link>
    </div>
  </div>
  <div class="content">
    <router-view />
  </div>
  <div class="footer">
  </div>
</div>
</template>

<style>
html {
  box-sizing: border-box;
}

body {
  font-family: 'Montserrat', sans-serif;
  font-size: 16px;
  background: #fff;
  padding: 0px;
  margin: 0px;
}

/* Header */
.header {
  display: flex;
  padding: 10px 100px 0px 100px;
  background-color: #348899;
  color: black;
}

.title {
  margin-top: 5px;
}

.title h1 {
  font-size: 30px;
}

.nav-bar {
  padding: 30px 100px 0px 100px;
}
.nav-button {
  color: white;
  padding-left: 20px;
  text-decoration: none;
}

.home-button {
  margin-right: 20px;
}
.post-page {
  margin-left: 20px;
}

.content {
  padding: 20px 100px;
  min-height: 500px;
}

/* Footer */
.footer {
  min-height: 250px;
  padding: 20px 100px 0px 100px;
  background-color: #979C9C;
  font-size: 12px;
}

.footer a {
  color: #000;
}

h1 {
  font-size: 20px;
}

h2 {
  font-size: 14px;
}
</style>
