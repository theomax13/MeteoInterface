<template>
  <div class="modal" id="modalModifNames" tabindex="-1">
    <div class="modal-dialog modal-dialog-centered">
      <div class="modal-content">
        <div class="modal-header">
          <h1 class="modal-title fs-5" id="modalLabel">
            Modification du nom du capteur
          </h1>
          <button
            type="button"
            class="btn-close"
            data-bs-dismiss="modal"
            aria-label="Close"
          ></button>
        </div>
        <div class="modal-body">
          <label for="recipient-name" class="col-form-label"
            >Nouveau nom:</label
          >
          <input
            v-model="newSensorName"
            class="form-control"
            id="newSensorName"
          />
        </div>
        <div class="modal-footer">
          <button
            type="button"
            class="btn btn-secondary"
            data-bs-dismiss="modal"
          >
            Close
          </button>
          <button
            @click="saveModifiedName"
            type="button"
            class="btn btn-success"
          >
            Save changes
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    sensorNames: Array,
  },
  data() {
    return {
      newSensorName: "",
      selectedSensor: null,
    };
  },
  methods: {
    openModal(sensor) {
      this.selectedSensor = sensor;
      this.newSensorName = sensor.nom_capteur || sensor.capteur;

      this.$("#modalModifNames").modal("show");
    },
    saveModifiedName() {
      if (this.selectedSensor) {
        this.$emit("updateSensorName", {
          capteur: this.selectedSensor.capteur,
          name: this.newSensorName,
        });
        this.closeModal();
      }
    },
    closeModal() {
      this.newSensorName = "";
      this.selectedSensor = null;

      this.$("#modalModifNames").modal("hide");
    },
  },
};
</script>

<!-- 
<script>
export default {
  props: {
    sensorNames: Array,
  },
  data() {
    return {
      newSensorName: "",
      showModifyDialog: false,
      selectedSensor: null,
    };
  },
  methods: {
    saveModifiedName() {
      if (this.selectedSensor) {
        this.$emit("updateSensorName", {
          capteur: this.selectedSensor.capteur,
          name: this.newSensorName,
        });
        this.showModifyDialog = false;
      }
    },
    modalId() {
      const modal = document.getElementById("modalModifNames");
      if (modal) {
        modal.addEventListener("show.bs.modal", (event) => {
          const button = event.relatedTarget;
          const recipient = button.getAttribute("data-bs-whatever");
          const modalTitle = modal.querySelector(".modal-title");
          const modalBodyInput = modal.querySelector(".modal-body input");
          modalTitle.textContent = `Modification du nom du capteur ${recipient}`;
          modalBodyInput.value = recipient;
          this.newSensorName = recipient;
          this.selectedSensor = this.sensorNames.find(
            (sensor) => sensor.capteur === recipient
          );
        });
      }
    },
  },
  mounted() {
    this.modalId();
  },
};
</script> -->
