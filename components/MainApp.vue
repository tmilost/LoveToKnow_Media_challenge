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
        <a href="/A.txt" class="link-to-files" download>Example file A.txt</a>
        <a href="/B.txt" class="link-to-files" download>Example file B.txt</a>
        <a href="/C.txt" class="link-to-files" download>Example file C.txt</a>
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

      <div class="sm:rounded-lg p-6">
        <el-card v-for="value in objTotalSubtext" :key="value" class="box-card">
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
      fileAaList: [],
      objInput: {},
      objTotalWithoutSubtext: {},
      objTotalSubtext: [],
      objTxtContainsSubText: {},
      reverseObject: [],
    };
  },
  methods: {
    readFile(file) {
      ///// It's all in one function, so if someone is looking at the code, they can do it faster

      let reader = new FileReader();
      reader.readAsText(file.raw);

      reader.onload = (e) => {
        var cont = e.target.result;

        cont = cont.split("\n").join("").split("\r");

        this.fileAaList.push(cont);

        var name = file.name;
        this.objInput[name] = cont;

        ////////// NEW FUNCTION ///////////
        ///// Proces Object, read array, separate total value and subtexts
        for (const [key, value] of Object.entries(this.objInput)) {
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

        ////////// NEW FUNCTION 1///////////
        ///// Add subtext of subtext
        for (const [key, value] of Object.entries(this.objTxtContainsSubText)) {
          if (value.length > 0) {
            value.forEach((element) => {
              if (this.objTotalWithoutSubtext[element] !== undefined) {
                this.objTxtContainsSubText[element].forEach((element1) => {
                  value.push(element1);
                });
                /// Remove duplicate values from subtext array
                let unique = [...new Set(value)];
                value = unique;
              }
            });
          }
        }

        ////////// NEW FUNCTION 2///////////
        ///// Count total with subtext
        for (const [key, value] of Object.entries(this.objTxtContainsSubText)) {
          if (value.length > 0) {
            value.forEach((element) => {
              if (this.objTotalWithoutSubtext[element] !== undefined) {
                this.objTotalWithoutSubtext[key] +=
                  this.objTotalWithoutSubtext[element];
              }
            });
          }
        }

        /////// Add Total value to array so it can be rendered
        this.objTotalSubtext = [];
        for (const [key, value] of Object.entries(
          this.objTotalWithoutSubtext
        )) {
          this.objTotalSubtext.push(key + " - " + value);
        }
      };
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
