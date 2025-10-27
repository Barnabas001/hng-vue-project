<template>
  <section class="ticket-page-section">
    <h2>Tickets</h2>

    <!-- CREATE / EDIT FORM -->
    <form class="form" @submit.prevent="handleSubmit">
      <label class="label">
        Title
        <input
          v-model.trim="form.title"
          class="input"
          placeholder="Enter ticket title"
          required
        />
      </label>
      <label class="label">
        Description
        <textarea
          v-model.trim="form.description"
          class="input"
          placeholder="Describe the issue..."
          rows="3"
          required
        ></textarea>
      </label>

      <label class="label">
        Status
        <select v-model="form.status" class="input">
          <option value="open">Open</option>
          <option value="resolved">Resolved</option>
          <option value="closed">Closed</option>
        </select>
      </label>

      <div class="form-action">
        <button class="button" type="submit">
          {{ editIndex !== null ? "Update Ticket" : "Add Ticket" }}
        </button>
        <button
          class="button"
          type="button"
          v-if="editIndex !== null"
          @click="cancelEdit"
        >
          Cancel Edit
        </button>
        <button class="button" type="button" @click="$emit('back')">
          Back to Dashboard
        </button>
      </div>
    </form>

    <!-- TICKETS TABLE -->
    <div class="ticket-page-table-container">
      <table class="ticket-page-table">
        <thead>
          <tr>
            <th class="ticket-page-th">ID</th>
            <th class="ticket-page-th">Title</th>
            <th class="ticket-page-th">Status</th>
            <th class="ticket-page-th">Actions</th>
          </tr>
        </thead>
        <tbody>
          <tr v-if="!tickets.length">
            <td colspan="4" style="padding: 12px">
              No tickets found. Add a new ticket above.
            </td>
          </tr>

          <tr v-for="(t, i) in tickets" :key="t.id">
            <td class="ticket-page-td">{{ t.id }}</td>
            <td class="ticket-page-td">{{ t.title }}</td>
            <td class="ticket-page-td">{{ t.status }}</td>
            <td class="ticket-page-td">
              <button class="button" @click="startEdit(i)">Edit</button>
              <button class="button" @click="deleteTicket(i)">Delete</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </section>
</template>

<script setup>
import { ref, watch } from "vue";

const props = defineProps({
  tickets: { type: Array, default: () => [] },
});
const emit = defineEmits(["update", "back"]);

const form = ref({
  title: "",
  description: "",
  status: "open",
});
const editIndex = ref(null);

/* --- METHODS --- */

// Add or update ticket
function handleSubmit() {
  if (!form.value.title.trim() || !form.value.description.trim()) return;

  const copy = [...props.tickets];

  if (editIndex.value !== null) {
    // Update existing
    copy[editIndex.value] = {
      ...copy[editIndex.value],
      ...form.value,
    };
  } else {
    // Create new
    const newTicket = {
      id: "t" + Date.now().toString().slice(-6),
      ...form.value,
      createdAt: Date.now(),
    };
    copy.push(newTicket);
  }

  emit("update", copy);
  resetForm();
}

// Start editing an existing ticket
function startEdit(index) {
  editIndex.value = index;
  form.value = {
    title: props.tickets[index].title,
    description: props.tickets[index].description,
    status: props.tickets[index].status,
  };
  window.scrollTo({ top: 0, behavior: "smooth" });
}

// Cancel edit mode
function cancelEdit() {
  resetForm();
}

// Delete a ticket
function deleteTicket(index) {
  if (confirm("Delete this ticket?")) {
    const copy = props.tickets.filter((_, i) => i !== index);
    emit("update", copy);
  }
}

// Reset form
function resetForm() {
  form.value = { title: "", description: "", status: "open" };
  editIndex.value = null;
}
</script>
