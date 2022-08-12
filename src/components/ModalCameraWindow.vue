<template>
  <div class="modal">
    <div class="modal--inner">
      <div class="modal--header">
        <h1>Set Video Stream</h1>
        <img @click="toggleModal()" src="../../public/assets/cancel.png" alt="cancel">
      </div>
      <hr class="modal--line" />

      <!-- div с полем для поиска -->
      <div class="modal--search">
        <h1>
          Search
        </h1>
        <input v-model="searchInput" type="text" @keyup="search">
      </div>

      <!-- div с группами и файлами -->
      <div class="modal--general">
        <div class="modal--general--files">
          <div class="modal--general--sort">
            <div class="name" @click="sortByName">
              <div class="arrow--imgs">
                <img v-if='isSortByName' src='../../public/assets/arrowUp.png' alt="arrowup">
                <img v-else src='../../public/assets/Polygon.png' alt="arrowup">

                <img v-if='!isSortByName' class="arrowDown" src="../../public/assets/arrowUp.png" alt="arrowdown">
                <img v-else class="arrowDown" src="../../public/assets/Polygon.png" alt="arrowdown">
              </div>
              <h1>Name</h1>
            </div>

            <div class="id" @click="sortByID">
              <div class="arrow--imgs">
                <img v-if='isSortByID' src='../../public/assets/arrowUp.png' alt="arrowup">
                <img v-else src='../../public/assets/Polygon.png' alt="arrowup">

                <img v-if='!isSortByID' class="arrowDown" src="../../public/assets/arrowUp.png" alt="arrowdown">
                <img v-else class="arrowDown" src="../../public/assets/Polygon.png" alt="arrowdown">
              </div>
              <h1>ID</h1>
            </div>
          </div>

          <div class="modal--general--groups" v-for="i in groupsArray" :key="i">
            <div class="modal--general--container">
              <div class="modal--pointer">
                <img src="../../public/assets/Vector.png" alt="arrow" @click="(e) => toggleClass(e)">
                <img src="../../public/assets/foldericon.png" alt="foldericon">
                <p>{{ i.name }}</p>
              </div>
            </div>
            <div ref="filesRef" class="hide" v-for="d in i.files" :key="d"
              @mouseover="onHoverFiles($event, d.city, d.street, d.timeUTC, d.size, d.codec, d.text, d.view)">
              <div class="modal--name--icon">
                <img src="../../public/assets/cameraicon.svg" alt="camera">
                <p>{{ d.name }}</p>
              </div>

              <div class="modal--idandcheckbox">
                <p>{{ d.id }}</p>
                <input @click="(e) => clickOnCheckbox(e)" ref="modalCheckbox" class="modal--checkbox" type="checkbox">
              </div>
            </div>
          </div>
          <div class="modal--general--filesDiv" v-for="i in files" :key="i.id"
            @mouseover="onHover($event, i.city, i.street, i.timeUTC, i.size, i.codec, i.text, i.view)">
            <div>
              <img src="../../public/assets/fileicon.png" alt="file">
              {{ i.name }}
            </div>

            <div class="modal--idandcheckbox">
              {{ i.id }}
              <input @click="(e) => clickOnCheckbox(e)" class="modal--checkbox" type="checkbox">
            </div>
          </div>
        </div>

        <!-- div с информацией справа -->
        <div class="modal--general--preview">
          <img class="zaglushka" src="../../public/assets/zaglushka.png" alt="zaglushka">
          <div class="modal--general--container">
            <div class="modal--general--div1">
              <p>Camera</p>
              <p>Address</p>
              <p>Timezone</p>
              <p>Resolution</p>
              <p>Codec</p>
              <p>Type</p>
            </div>

            <div class="modal--general--div2">
              <p>{{ cityRef }} <img src="../../public/assets/placeicon.png" alt="place"></p>
              <p>{{ streetRef }}</p>
              <p>{{ timeUTCRef }}</p>
              <p>{{ sizeRef }}</p>
              <p>{{ codecRef }}</p>
              <p>{{ textRef }}</p>
            </div>
          </div>
        </div>
      </div>

      <div class="modal--btn">
        <button>
          Set Video Stream
        </button>
      </div>
    </div>
  </div>
</template>

<script>
//imports
import root from "@/root.js"
import { ref } from "vue"

export default {
  props: ["toggleModal"],
  //use composition API and vue 3
  setup() {
    const files = ref(root.root.files)
    const groupsArray = ref(root.root.groups)
    const filesRef = ref()
    const cityRef = ref("")
    const streetRef = ref("")
    const timeUTCRef = ref("")
    const sizeRef = ref("")
    const codecRef = ref("")
    const textRef = ref("")
    const viewRef = ref("")
    const modalCheckbox = ref(false)
    const searchInput = ref("")
    const isSortByID = ref(false)
    const isSortByName = ref(false)

    //Метод которая при наведении на камеры записывает данные в переменные для файлов
    function onHoverFiles(e, city, street, time, size, codec, text, view) {
      cityRef.value = city
      streetRef.value = street
      timeUTCRef.value = time
      sizeRef.value = size
      codecRef.value = codec
      textRef.value = text
      viewRef.value = view
    }

    //Метод которая при наведении на камеры записывает данные в переменные для файлов которые находяться в группе
    function onHover(e, city, street, time, size, codec, text, view) {
      if (e.target.classList.contains("modal--general--filesDiv")) {
        cityRef.value = city
        streetRef.value = street
        timeUTCRef.value = time
        sizeRef.value = size
        codecRef.value = codec
        textRef.value = text
        viewRef.value = view
      } else if (e.target.parentNode.classList.contains("modal--general--filesDiv")) {
        cityRef.value = city
        streetRef.value = street
        timeUTCRef.value = time
        sizeRef.value = size
        codecRef.value = codec
        textRef.value = text
        viewRef.value = view
      }
    }

    // Checkbox click method
    function clickOnCheckbox(e) {
      e.target.parentNode.parentNode.classList.toggle("select--checkbox")
    }
    
    //Метод сортировки по имени 
    function sortByName() {
      if (isSortByName.value) {
        for (let i = 0; i < groupsArray.value.length; i++) {
        groupsArray.value[i].files.sort((a, b) => {
          return b.name.localeCompare(a.name)
        })
        }

        groupsArray.value.sort((a, b) => {
          return b.name.localeCompare(a.name)
        })

        files.value.sort((a, b) => {
          return b.name.localeCompare(a.name)
        })
        isSortByName.value = false
      } else {
        for (let i = 0; i < groupsArray.value.length; i++) {
        groupsArray.value[i].files.sort((a, b) => {
          return a.name.localeCompare(b.name)
        })
        }

        groupsArray.value.sort((a, b) => {
          return a.name.localeCompare(b.name)
        })

        files.value.sort((a, b) => {
          return a.name.localeCompare(b.name)
        })
        isSortByName.value = true
      }
    }

    //Метод сортировки по ID 
    function sortByID() {
      if (isSortByID.value) {
        for (let i = 0; i < groupsArray.value.length; i++) {
        groupsArray.value[i].files.sort((a, b) => {
          return b.id - a.id
        })
        }
        groupsArray.value.sort((a, b) => {
          return b.files.id - a.files.id
        })
        files.value.sort((a, b) => {
          return b.id - a.id
        })
        isSortByID.value = false
      } else {
        for (let i = 0; i < groupsArray.value.length; i++) {
        groupsArray.value[i].files.sort((a, b) => {
          return a.id - b.id
        })
        }
        groupsArray.value.sort((a, b) => {
          return a.files.id - b.files.id
        })
        files.value.sort((a, b) => {
          return a.id - b.id
        })
        isSortByID.value = true
      }
    }

    //Метод для поиска 
    function search() {
      const groupsSearch = root.root.groups
      const filesSearch = root.root.files

      // Не вышло реализовать поиск по файлам в групппе 
      // for (let i = 0; i < groupsSearch.length; i++) {
      //   const result = groupsSearch[i].files.filter(t => t.name.toLowerCase().includes(searchInput.value.toLowerCase()))
      //   console.log(result)
      // }

      const groupResult = groupsSearch.filter(t => t.name.toLowerCase().includes(searchInput.value.toLowerCase()))
      const fileResult = filesSearch.filter(t => t.name.toLowerCase().includes(searchInput.value.toLowerCase()))
      files.value = fileResult
      groupsArray.value = groupResult
    }

    //Метод который отвечает за сворачивание и разворачивание груп
    function toggleClass(e) {
      if (e.target.parentNode.classList.contains("modal--general--container")) {
        e.target.parentNode.classList.toggle("active")
        e.target.classList.toggle("rotate")
      } else if (e.target.parentNode.parentNode.classList.contains("modal--general--container")) {
        e.target.parentNode.parentNode.classList.toggle("active")
        e.target.classList.toggle("rotate")
      }
    }
    return {
      root,
      groupsArray,
      files,
      toggleClass,
      filesRef,
      onHover,
      cityRef,
      streetRef,
      timeUTCRef,
      sizeRef,
      codecRef,
      textRef,
      viewRef,
      onHoverFiles,
      modalCheckbox,
      sortByName,
      sortByID,
      search,
      searchInput,
      isSortByID,
      isSortByName,
      clickOnCheckbox
    }
  }
}
</script>

<style scoped lang="scss">
.modal {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  z-index: 99;
  background-color: rgb(0, 0, 0, 0.5);

  display: flex;
  justify-content: center;
  align-items: center;
  padding: 30px;

  .modal--inner {
    width: 800px;
    height: 630px;
    background-color: #0f1115;
    border: 1px solid rgb(255, 255, 255, 0.5);
    border-radius: 5px;

    .modal--general--preview {
      .zaglushka {
        width: 312px;
        height: 179px;
      }
    }

    h1 {
      font-size: 24px;
      font-weight: 500;
      margin: 0;
    }

    .modal--header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 30px 30px 20px 30px;

      img {
        cursor: pointer;
      }
    }

    .modal--general--container {
      display: flex;

      p {
        padding: 6px 6px 6px 0;
        margin: 0;
        display: flex;
        align-items: center;

        img {
          padding-top: 2px;
        }
      }

      .modal--general--div1 {
        p {
          color: #686A70;
          font-size: 12px;
          font-weight: 300;
        }
      }

      .modal--general--div2 {
        p:nth-child(1) {
          padding-top: 2px;
        }
        p:nth-child(2) {
          padding-top: 3px;
          padding-bottom: 6px;
        }
        p:nth-child(4) {
          padding-top: 6px;
          padding-bottom: 8px;
        }

        p {
          padding-left: 4px;
          padding-top: 5px;
          color: #C0C0C0;
        }
      }
    }
  }
  .modal--idandcheckbox {
    display: flex;
    align-items: center;
  }
  .modal--line {
    width: 92%;
    opacity: 20%;
    margin-bottom: 20px;
  }

  .modal--search {
    padding: 0 30px;

    h1 {
      font-size: 14px;
      color: #686A70;
      font-weight: 300;
      padding-bottom: 6px;
    }

    input {
      width: 396px;
      height: 30px;
      border-radius: 10px;
      border: 1px solid rgb(255, 255, 255, 0.5);
      background-color: #23252c;
      margin-bottom: 20px;
      color: white;
      padding-left: 10px;
    }
  }

  .modal--general {
    display: flex;

    padding: 0 30px;
    margin-bottom: 30px;

    .modal--general--files {
      width: 410px;
      height: 360px;
      border: 1px solid rgb(255, 255, 255, 0.5);
      border-radius: 5px;
      margin-right: 20px;
      overflow: auto;

      .modal--general--groups {

        .modal--checkbox {
          margin-right: 10px;
        }

        .modal--pointer {
          display: flex;
          align-items: center;
          cursor: pointer;

          img {
            transition: all 0.2s;
          }
        }

        .modal--general--container {
          display: flex;
          align-items: center;
          justify-content: space-between;

          img:nth-child(1) {
            width: 10px;
            height: 6px;

            padding: 0 13px 0 13px;
            transform: rotate(90deg);
          }

          img:nth-child(2) {
            width: 12px;
            height: 12px;
          }

          input {
            margin-left: 58px;
          }

          p {
            margin: 0;
            padding: 0;
            padding-left: 5px;
          }
        }

        .modal--name--icon {
          display: flex;
          align-items: center;
          cursor: pointer;

          margin-left: 30px;

          img {
            width: 12px;
            height: 12px;
          }
        }

        .hide {
          display: none;
          align-items: center;
          justify-content: space-between;

          img {
            padding: 0px 2px 0 25px;
          }

          .modal--checkbox {
            margin-right: 10px;
            margin-left: 58px;
          }

          div:nth-child(2) {
            display: flex;
          }

          p {
            margin: 0;
            padding: 0;
            padding-left: 5px;
          }
        }

        .active~.hide {
          display: flex;
        }
      }

      .modal--general--filesDiv {
        display: flex;
        align-items: center;
        justify-content: space-between;
        cursor: pointer;

        p {
          margin: 0;
          padding: 0;
          padding-left: 5px;
        }

        .modal--checkbox {
          margin-right: 10px;
          margin-left: 58px;
        }

        img {
          padding: 0 13px 0 13px;
        }
      }

      .modal--general--sort {
        display: flex;
        justify-content: space-around;
        padding-top: 13px;
        padding-bottom: 20px;

        .name,
        .id {
          display: flex;
          align-items: center;
          cursor: pointer;
        }

        .arrow--imgs {
          display: flex;
          flex-direction: column;

          img:nth-child(1) {
            padding-bottom: 2px;
          }
        }

        h1 {
          font-size: 15px;
          color: #C0C0C0;
          font-weight: 500;
          padding-left: 10px;
        }
      }
    }

    .arrowDown {
      transform: rotate(-180deg);
    }
  }

  .modal--btn {
    display: flex;
    justify-content: center;
    align-items: center;

    button {
      font-family: 'Inter', sans-serif;
      text-transform: uppercase;
      width: 225px;
      height: 40px;
      cursor: pointer;
      border: 1px solid white;
      background: none;
      color: white;
      border-radius: 20px;
      color: #3FD5B8;
      border: 1px solid #3FD5B8;
      transition: 0.3s all;
    }

    button:hover {
      background-color: #3FD5B8;
      color: #FFF;
    }
  }
}
//Class rotate for arrows
.rotate {
  transform: rotate(180deg) !important;
}
.select--checkbox {
  color: #3FD5B8;
}

//Styling scrollbar and thumb
.modal--general--files::-webkit-scrollbar-track {
  padding: 2px 0;
  background-color: #686A70;
}

.modal--general--files::-webkit-scrollbar {
  width: 8px;
}

.modal--general--files::-webkit-scrollbar-thumb {
  border-radius: 0 5px 5px 0;
  box-shadow: inset 0 0 6px rgba(0, 0, 0, .3);
  background-color: #3FD5B8;
}

//General styles for checkboxes
input[type="checkbox"] {
  -webkit-appearance: none;
  width: 12px;
  height: 12px;
  background: #0f1115;
  border: 1px solid #686A70;
  
	position: relative;
}
input[type="checkbox"]:active, input[type="checkbox"]:checked:active {
	box-shadow: 0 1px 2px rgba(0,0,0,0.05), inset 0px 1px 3px rgba(0,0,0,0.1);
}

input[type="checkbox"]:checked {
	background-color: #fff;
	box-shadow: 0 1px 2px rgba(0,0,0,0.05), inset 0px -15px 10px -12px rgba(0,0,0,0.05), inset 15px 10px -12px rgba(255,255,255,0.1);
	color: #99a1a7;
}
input[type="checkbox"]:checked:after {
	content: '\AC';
	font-size: 16px;
  font-weight: 900;
  top: -7px;
	position: absolute;
	color: #686A70;
  transform: rotate(130deg);
}
</style>
