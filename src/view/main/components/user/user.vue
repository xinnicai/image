<template>
  <div class="user-avator-dropdown">
    <Dropdown @on-click="handleClick">
      <Avatar :src="userAvator"/>
      <Icon :size="18" type="md-arrow-dropdown"></Icon>
      <DropdownMenu slot="list">
        <DropdownItem name="logout">退出登录</DropdownItem>
      </DropdownMenu>
    </Dropdown>
  </div>
</template>

<script>
import './user.less'
import { mapActions } from 'vuex'
export default {
  name: 'User',
  props: {
    userAvator: {
      type: String,
      default: 'https://file.iviewui.com/dist/a0e88e83800f138b94d2414621bd9704.png'
    }
  },
  methods: {
    ...mapActions([
      'handleLogOut'
    ]),
    handleClick (name) {
      this.$router.push({
        name: 'login'
      })
      // switch (name) {
      //   case 'logout':
      //     this.handleLogOut().then(() => {
      //       this.$router.push({
      //         name: 'login'
      //       })
      //     })
      //     break
      // }
    }
  }
}
</script>
