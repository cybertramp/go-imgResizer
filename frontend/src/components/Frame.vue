<template>
  <div>
    <h1>이미지 비율 조절</h1>
  </div>
  <p></p>
  <div>
    <input type='file' id='input' accept="image/*" @change="file_selected">
  </div>
  <div>
    <p>
      원래 크기: {{ datas.img_w }}px X {{ datas.img_h }}px
      <br>
    </p>
    <p>[옵션]</p>
    <p>
      조정될 크기: {{ datas.img_w_save }}px X {{ datas.img_h_save }}px
    </p>
    <p>
      높이(px) <input type='number' v-model="datas.img_h_save">
      <br>
      너비(px) <input type='number' v-model="datas.img_w_save">
      <br>
      저장 타입 <select v-model="datas.select_save_type" name="저장 타입">
        <option
        v-for="(item, index) in datas.select_save_list"
        :key="index"
        :value="item.name"
        >{{ item.name }}</option>
        </select>
    </p>
    <button @click="file_download(datas.img_url)">파일로 저장</button>

  </div>
  <div>
    <p></p>
  </div>

  <div class="image-box">
    <p>
      <img :src="datas.img_url" width="320" height="240" />
    </p>
    <p>
      <canvas id="canvas" class="canvas"></canvas>
    </p>
  </div>

</template>

<script lang="ts">
import { reactive, onMounted } from 'vue';
export default ({

  setup() {
    const datas = reactive({
      img_url: '',
      img_filename: 'undefined.png',
      img_h: 0,
      img_w: 0,
      img_h_save: 0,
      img_w_save: 0,
      select_save_type: 'png',
      select_save_list: [
        {
          id: 0,
          name: 'png'
        },
        {
          id: 1,
          name: 'jpg'
        },
      ],
    });

    onMounted(() => {
      console.log("Mounted!");
    });


    const file_selected = (e: Event) => {
      e.preventDefault();

      const target = e.target as HTMLInputElement;
      const files = target.files;

      if (files != null) {
        datas.img_filename = files[0].name.slice(0,-4) + "_rev";
        
        const objectUrl = window.URL.createObjectURL(files[0]);
        if (objectUrl) {
          var img = new Image;

          img.src = objectUrl;
          datas.img_url = objectUrl;

          img.onload = async function (e) {

            if (img.height == img.width) {
              alert("참고: 이 파일은 이미 1:1 비율입니다.");
            }

            datas.img_h = img.height;
            datas.img_w = img.width;

            datas.img_h_save = img.width;
            datas.img_w_save = img.width;

          }
        }
      }
    }

    const file_download = (url: string, filename = 'untitled.jpeg') => {
      console.log(datas.img_url)
      var img = new Image;
      var canvas = document.createElement('canvas')
      const ctx = canvas?.getContext('2d');

      if (ctx != null && canvas != null) {
        img.src = url;
        img.onload = async function (e) {
          canvas.width = datas.img_w_save;
          canvas.height = datas.img_h_save;
          ctx.drawImage(img, 0, 0, datas.img_w_save, datas.img_h_save);

          var a = document.createElement('a');
          var save_type: string = 'png';
          if (datas.select_save_type === 'jpg') {
            save_type = "image/jpeg";
          } else if (datas.select_save_type === 'png') {
            save_type = "image/png";
          }
          a.href = ctx.canvas.toDataURL(save_type, 1.0);
          a.download = datas.img_filename;
          document.body.appendChild(a);
          a.click();

        }
      }
    }

    return {
      // functions
      datas,
      file_selected,
      file_download,
    }
  },
});

</script>


<style scoped>
.read-the-docs {
  color: #888;
}

.image-box {
  text-align: center;
}

canvas {
  visibility: hidden;
}
</style>