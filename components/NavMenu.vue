<template>
  <div>
    <el-menu class="el-menu-demo" mode="horizontal">
      <div class="flex justify-center">
        <el-menu-item
          style="float: left"
          v-on:click="toLink(config.origintrailUrl)"
        >
          <img
            src="~/assets/loveToKnow.png"
            alt="loveToKnow"
            width="65"
            height="40px"
          />
        </el-menu-item>
        <el-menu-item
          v-on:click="toLink(config.linkedInUrl)"
          style="float: right; width: 59px"
          ><font-awesome-icon :icon="['fab', 'linkedin']" />
        </el-menu-item>

        <el-menu-item
          v-on:click="toLink(config.gitUrl)"
          style="float: right; width: 59px"
          ><font-awesome-icon :icon="['fab', 'github']" />
        </el-menu-item>
        <el-menu-item
          v-on:click="toLink(config.instagramUrl)"
          style="float: right; width: 59px"
          ><font-awesome-icon :icon="['fab', 'instagram']" />
        </el-menu-item>
        <el-menu-item
          v-on:click="toLink(config.cvUrl)"
          style="float: right; width: 59px"
        >
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
</style>