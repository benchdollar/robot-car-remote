<script>

//export const apiUrl = "http://localhost:3004/command"
export const apiUrl = "http://192.168.178.186/command"

export const driveMotion = {
    STOP: 'STOP',
    FORWARDS: 'FORWARDS',
    BACKWARDS: 'BACKWARDS',
    ROTATE_LEFT: 'ROTATE_LEFT',
    ROTATE_RIGHT: 'ROTATE_RIGHT'
};

export default {
  data() {
    return {
      status: driveMotion.STOP,
      display: '',
      driveMotion: driveMotion
    }
  },
//   mounted() {
//     this.display = driveMotion.STOP
//   },
  methods: {
    drive(direction) {
        this.status = direction
        this.issueCommand(direction)
    },
    async issueCommand(driveMotion) {
        const requestOptions = {
            method: 'POST',
            body: driveMotion
        }
        fetch(apiUrl, requestOptions)
        .then( async response => {
            const data = await response.text();
            if(!response.ok) {
                return Promise.reject("ERROR " + response.status.toString)
            }

            this.status = driveMotion
            this.display = driveMotion + " (response: " + data + ")"
            console.log("Successful API call", this.display)
        })
        .catch(error => {
            this.status = this.driveMotion.STOP
            this.display = error.toString
            console.error("API error", error)
        })
       
    }
  },
//   watch: {
//     status(newStatus) {
//         this.issueCommand(newStatus)
//     }
//   }
}
</script>

<template>
  <div class="control">
  <button @click="drive(driveMotion.FORWARDS)" :class="{ active: status === driveMotion.FORWARDS }">&UpArrow;</button><br/>
  <button @click="drive(driveMotion.ROTATE_LEFT)" :class="{ active: status === driveMotion.ROTATE_LEFT }">&circlearrowleft;</button>
  <button @click="drive(driveMotion.STOP)" :class="{ active: status === driveMotion.STOP }">&FilledSmallSquare;</button>
  <button @click="drive(driveMotion.ROTATE_RIGHT)" :class="{ active: status === driveMotion.ROTATE_RIGHT }">&circlearrowright;</button><br/>
  <button @click="drive(driveMotion.BACKWARDS)" :class="{ active: status === driveMotion.BACKWARDS }">&DownArrow;</button>
  </div>
  <p class="display">{{status}}</p>
</template>

<style scoped>
.control {
    margin-top: 32px;
    text-align: center;
}
button {
  font-weight: bold;
  min-width: 80px;
  min-height: 80px;
  margin: 4px;
  font-size: x-large;
}

.active {
    background-color: yellow;
}

.display {
    margin: 16px;
    text-align: center;
    font-weight: bold;
}
</style>