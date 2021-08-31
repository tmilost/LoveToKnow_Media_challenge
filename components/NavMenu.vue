<template>
  <div>
    <el-menu class="el-menu-demo" mode="horizontal">
      <div class="flex justify-center">
        <el-menu-item
          class="menu-item-image"
          v-on:click="toLink(config.origintrailUrl)"
        >
          <img
            src="~/assets/loveToKnow.png"
            alt="loveToKnow"
            width="65"
            height="40px"
          />
        </el-menu-item>
        <el-menu-item v-on:click="toLink(config.linkedInUrl)" class="menu-item"
          ><font-awesome-icon :icon="['fab', 'linkedin']" />
        </el-menu-item>

        <el-menu-item v-on:click="toLink(config.gitUrl)" class="menu-item"
          ><font-awesome-icon :icon="['fab', 'github']" />
        </el-menu-item>
        <el-menu-item v-on:click="toLink(config.instagramUrl)" class="menu-item"
          ><font-awesome-icon :icon="['fab', 'instagram']" />
        </el-menu-item>
        <el-menu-item v-on:click="toLink(config.cvUrl)" class="menu-item">
          <font-awesome-icon :icon="['fas', 'address-card']" />
        </el-menu-item>
      </div>
    </el-menu>
  </div>
</template>

<script>
import config from "~/assets/config.json";
export default {
  data: () => ({
    config: config,
  }),
  methods: {
    toLink(adress) {
      window.location.href = adress;
    },
  },
};
</script>

<style>
.el-menu-item {
  width: 575px;
}
.menu-item-image {
  float: left;
}
.menu-item {
  float: right;
  width: 59px;
}
</style>