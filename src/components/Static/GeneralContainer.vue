<template>
  <div class="general">
    <header class="general--header">
      <h1 class="general--header--title">
        Add Smart VA Analytics
      </h1>
      <div class="general--header--components">
        <img src="../../../public/assets/component1.png" alt="component">
        <img src="../../../public/assets/Photo.png" alt="photo">
      </div>
    </header>
    <main class="general--main">
      <div class="main--header">
        <div class="main--header--imgh1">
          <img src="../../../public/assets/ellipse.png" alt="ellipse">
          <h1>General</h1>
        </div>
        <div class="main--header--buttons">
          <button @click="() => toggleModal('buttonTrigger')">back</button>
          <button @click="() => toggleModal('buttonTrigger')">next</button>
        </div>
      </div>
      <div class="main--staticdiv">
        <div class="main--staticdiv--videostream">
          <div>
            <h1>Video stream</h1>
            <img @click="() => toggleModal('buttonTrigger')" src="../../../public/assets/component2.png" alt="component2">
          </div>
          <select>
            <option value="Static">Not chosen</option>
            <option value="Static">Static</option>
            <option value="Static">Static</option>
          </select>
          <hr class="main--line" />
        </div>
        <div class="main--staticdiv--recordingoptions">
          <h1>Recording Options</h1>
          <div>
            <p>No zones selected</p>
          </div>
        </div>
      </div>
    </main>
  </div>
  <ModalCameraWindow v-if="popupTrigger.buttonTrigger" :toggleModal = "() => toggleModal('buttonTrigger')"/>
</template>

<script>
import { ref } from 'vue'
import ModalCameraWindow from "../ModalCameraWindow.vue"

export default {
  components: {
    ModalCameraWindow,
  },
  setup() {
    const popupTrigger = ref({
      buttonTrigger: false
    })
    const toggleModal = (trigger) => {
      popupTrigger.value[trigger] = !popupTrigger.value[trigger]
    }

    return {
      popupTrigger,
      toggleModal,
    }
  }
}
</script>

<style scoped lang="scss">
.general {
  width: 100%;

  background-color: black;
  
  padding: 30px 25px 0 25px;

  .general--header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding-bottom: 70px;

    .general--header--title {
      font-size: 24px;
      font-weight: 500;
    }
    .general--header--components {
      display: flex;
      justify-content: space-between;
      width: 475px;
    }
  }

  .general--main {
    .main--header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding-bottom: 25px;

      .main--header--imgh1 {
        display: flex;
        justify-content: space-between;
        align-items: center;
        width: 140px;

        h1 {
          font-size: 20px;
          font-weight: 400;
        }
      }

      .main--header--buttons {
        width: 220px;
        display: flex;
        justify-content: space-between;

        button {
          font-family: 'Inter', sans-serif;
          text-transform: uppercase;
          width: 100px;
          height: 30px;
          cursor: pointer;
          border: 1px solid white;
          background: none;
          color: white;
          border-radius: 20px;
          transition: 0.3s all;
        }
        button:nth-child(1) {
          color: #E2782B;
          border: 1px solid #E2782B;
        }
        button:nth-child(1):hover {
          background-color: #E2782B;
          color: #FFF;
        }
        button:nth-child(2) {
          color: #3FD5B8;
          border: 1px solid #3FD5B8;
        }
        button:nth-child(2):hover {
          background-color: #3FD5B8;
          color: #FFF;
        }
      }
    }
    .main--staticdiv {
      height: 400px;
      border: 1px solid rgb(255, 255, 255, 0.3);
      border-radius: 5px;
      background-color: #0f1115;
      display: flex;
      padding: 25px;

      .main--staticdiv--videostream {
        margin-right: 25px;
        width: 50%;
        div {
          width: 100%;
          height: 30px;
          
          display: flex;
          justify-content: space-between;
          align-items: center;

          padding-bottom: 15px;

          h1 {
            font-size: 14px;
            font-weight: 400;
            color: #686A70;
          }
          img {
            width: 163px;
            height: 13px;
            cursor: pointer;
          }
        }
        select {
          background: none;
          border: none;
          color: #686A70;
          width: 100%;
          outline: none;
          option {
            padding: 0;
            margin: 0;
          }
        }
        .main--line {
          width: 99%;
          border: 1px solid #686A70;
        }
      }
      .main--staticdiv--recordingoptions {
        width: 45%;
        h1 {
          font-size: 14px;
          font-weight: 400;
          color: #686A70;
        }
        div {
          width: 100%;
          border-radius: 5px;
          border: 1px solid #686A70;
          padding: 5px;
          p {
            color: #686A70;
            margin-left: 10px;
          }
        }
      }
    }
  }
}
</style>