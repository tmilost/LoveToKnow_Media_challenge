<!-- Please remove this file from your project -->
<template>
  <div
    class="
      relative
      flex
      items-top
      justify-center
      min-h-screen
      bg-gray-100
      sm:pt-0
    "
  >
    <link
      href="https://cdn.jsdelivr.net/npm/tailwindcss@2.1.2/dist/tailwind.min.css"
      rel="stylesheet"
    />
    <div class="max-w-4xl mx-auto sm:px-6 lg:px-8">
      <div class="download-files">
        <b class="link-to-files">Click on example file to download</b>
        <a :href="urlToFile('/A.txt')" class="link-to-files" download
          >Example file A.txt</a
        >
        <a :href="urlToFile('/B.txt')" class="link-to-files" download
          >Example file B.txt</a
        >
        <a :href="urlToFile('/C.txt')" class="link-to-files" download
          >Example file C.txt</a
        >
      </div>
      <div class="flex justify-center pt-8 sm:pt-0">
        <el-upload
          class="upload-demo"
          drag
          action="https://jsonplaceholder.typicode.com/posts/"
          :on-change="readFile"
          accept=".txt"
          multiple
        >
          <i class="el-icon-upload"></i>
          <div class="el-upload__text">
            Drop file here or <em>click to upload</em>
          </div>
          <div class="el-upload__tip" slot="tip">
            .txt files with a size less than 2GB
          </div>
        </el-upload>
      </div>
      <div class="flex justify-center pt-8 sm:pt-0 my-2">
        <el-dropdown>
          <span class="el-dropdown-link">
            Choose which file to start from<i
              class="el-icon-arrow-down el-icon--right"
            ></i>
          </span>
          <el-dropdown-menu slot="dropdown">
            <el-dropdown-item
              v-for="(value, name) in objInput"
              :key="name"
              @click.native="sumFromFile(name)"
              >{{ name }}</el-dropdown-item
            >
          </el-dropdown-menu>
        </el-dropdown>
      </div>
      <div class="sm:rounded-lg p-6">
        <el-card
          v-for="value in arrayTotalSubtext"
          :key="value"
          class="box-card"
        >
          <div class="text item">{{ value }}</div>
        </el-card>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data: function () {
    return {
      objInput: {},
      objTotalWithoutSubtext: {},
      objTxtContainsSubText: {},
      objTotalWithSubText: {},
      arrayTotalSubtext: [],
    };
  },
  methods: {
    urlToFile(addres) {
      let fullAdress = window.location.href + addres;
      return fullAdress;
    },
    readFile(file) {
      ///// It's all in one function, so if someone is looking at the code, they can do it faster

      let reader = new FileReader();
      reader.readAsText(file.raw);

      reader.onload = (e) => {
        var cont = e.target.result;

        cont = cont.split(/\r\n|\n/);

        var name = file.name;
        this.objInput[name] = cont;

        ////////// NEW FUNCTION ///////////
        ///// Proces Object, read array, separate total value and subtexts

        this.separateTotalAndSubtext(this.objInput);

        ////////// NEW FUNCTION 1///////////
        ///// Add subtext of subtext

        this.addSubTextOfOther();

        ////////// NEW FUNCTION 2///////////
        ///// Count total with subtext

        this.countTotal();

        /////// Add Total value to array so it can be rendered

        this.addTotalToArray();
      };
    },
    separateTotalAndSubtext(objInput) {
      for (const [key, value] of Object.entries(objInput)) {
        var subTextArray = [];
        var total = 0;
        value.forEach((element) => {
          if (element.match(/^\d+$/)) {
            total += parseInt(element);
          } else if (element.match(/.txt$/)) {
            subTextArray.push(element);
          }
          this.objTxtContainsSubText[key] = subTextArray;

          this.objTotalWithoutSubtext[key] = total;
        });
      }
    },
    addSubTextOfOther() {
      for (const [key, value] of Object.entries(this.objTxtContainsSubText)) {
        if (value.length > 0) {
          value.forEach((arrayOfSubText) => {
            if (this.objTotalWithoutSubtext[arrayOfSubText] !== undefined) {
              this.objTxtContainsSubText[arrayOfSubText].forEach((subText) => {
                value.push(subText);
              });
              /// Remove duplicate values from subtext array
              let unique = [...new Set(value)];
              value = unique;
            }
          });
        }
        this.objTotalWithSubText = Object.assign(
          {},
          this.objTotalWithoutSubtext
        );
      }
    },
    countTotal() {
      for (const [key, value] of Object.entries(this.objTxtContainsSubText)) {
        if (value.length > 0) {
          value.forEach((subText) => {
            if (this.objTotalWithoutSubtext[subText] !== undefined) {
              this.objTotalWithSubText[key] +=
                this.objTotalWithoutSubtext[subText];
            }
          });
        }
      }
    },
    addTotalToArray() {
      this.arrayTotalSubtext = [];
      for (const [key, value] of Object.entries(this.objTotalWithSubText)) {
        this.arrayTotalSubtext.push(key + " - " + value);
      }
    },
    sumFromFile(name) {
      var objInputCopy = Object.assign({}, this.objInput);
      var index = Object.keys(this.objInput).indexOf(name);
      for (const [key, value] of Object.entries(this.objInput)) {
        var aa = Object.keys(this.objInput).indexOf(key);
        if (aa < index) {
          delete objInputCopy[key];
        }
      }

      this.objTotalWithoutSubtext = {};
      this.objTxtContainsSubText = {};
      this.objTotalWithSubText = {};
      this.arrayTotalSubtext = [];

      this.separateTotalAndSubtext(objInputCopy);
      this.addSubTextOfOther();
      this.countTotal();
      this.addTotalToArray();
    },
  },
};
</script>

<style scoped>
.upload-demo {
  margin-top: 35px;
}
.box-card {
  min-width: 165px;
  float: left;
  margin: 15px 23px 20px 0px;
}
.download-files {
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
}
.link-to-files {
  font-size: 12px;
  color: #606266;
  margin-top: 9px;
}
</style>

