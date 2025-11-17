<script>
  import { createEventDispatcher } from 'svelte';

  const DAYS = ['Mon', 'Tue', 'Wed', 'Thu', 'Fri'];

  export let availableCourses = [];

  const dispatch = createEventDispatcher();

  let selectedCourseKey = '';
  let name = '';
  let location = '';
  let selectedDays = ['Mon', 'Wed'];
  let startTime = '09:00';
  let endTime = '10:00';
  let error = '';

  function toggleDay(day) {
    if (selectedDays.includes(day)) {
      selectedDays = selectedDays.filter((d) => d !== day);
    } else {
      selectedDays = [...selectedDays, day];
    }
  }

  function handleCourseChange() {
    const course = availableCourses.find((c) => c.key === selectedCourseKey);
    if (!course) return;

    name = course.name;
    location = course.location;
    selectedDays = [...course.defaultDays];
    startTime = course.defaultStart;
    endTime = course.defaultEnd;
  }

function handleSubmit(e) {
  e.preventDefault();
  error = '';

  if (!name.trim()) {
    error = 'Please enter a course name.';
    return;
  }
  if (selectedDays.length === 0) {
    error = 'Please select at least one day.';
    return;
  }
  if (startTime >= endTime) {
    error = 'End time must be after start time.';
    return;
  }

  // FIX: trim all day values
  const normalizedDays = selectedDays.map(d => d.trim());

  dispatch('addCourse', {
    name: name.trim(),
    location: location.trim(),
    days: normalizedDays,
    startTime,
    endTime
  });

  name = '';
  location = '';
}

</script>

<form class="course-form" on:submit|preventDefault={handleSubmit}>
  <div class="field">
    <label for="course-select">Select Existing Course</label>
    <select
      id="course-select"
      bind:value={selectedCourseKey}
      on:change={handleCourseChange}
    >
      <option value="">-- Choose a course --</option>
      {#each availableCourses as course}
        <option value={course.key}>{course.name}</option>
      {/each}
    </select>
  </div>

  <div class="field">
    <label for="course-name">Course Name</label>
    <input
      id="course-name"
      bind:value={name}
      placeholder="e.g. Data Structures"
    />
  </div>

  <div class="field">
    <label for="course-location">Location</label>
    <input
      id="course-location"
      bind:value={location}
      placeholder="e.g. CS 201"
    />
  </div>

  <div class="field">
    <span class="field-label">Days</span>
    <div class="days">
      {#each DAYS as day}
        <button
          type="button"
          class:selected={selectedDays.includes(day)}
          on:click={() => toggleDay(day)}
        >
          {day}
        </button>
      {/each}
    </div>
  </div>

  <div class="field time-row">
    <div>
      <label for="start-time">Start Time</label>
      <input id="start-time" type="time" bind:value={startTime} />
    </div>
    <div>
      <label for="end-time">End Time</label>
      <input id="end-time" type="time" bind:value={endTime} />
    </div>
  </div>

  {#if error}
    <p class="error">{error}</p>
  {/if}

  <button class="primary" type="submit">Add Class</button>
</form>

<style>
  .course-form {
    display: flex;
    flex-direction: column;
    gap: 0.75rem;
  }
  .field {
    display: flex;
    flex-direction: column;
    gap: 0.25rem;
  }
  label,
  .field-label {
    font-size: 0.85rem;
    font-weight: 600;
  }
  input,
  select {
    padding: 0.35rem 0.5rem;
    border-radius: 0.4rem;
    border: 1px solid #ccc;
    font-size: 0.9rem;
  }
  .days {
    display: flex;
    flex-wrap: wrap;
    gap: 0.35rem;
  }
  .days button {
    border-radius: 999px;
    border: 1px solid #999;
    padding: 0.25rem 0.6rem;
    font-size: 0.8rem;
    background: #f8f8f8;
    cursor: pointer;
  }
  .days button.selected {
    background: #2563eb;
    color: white;
    border-color: #2563eb;
  }
  .time-row {
    display: flex;
    gap: 0.75rem;
  }
  .time-row > div {
    flex: 1;
  }
  .primary {
    align-self: flex-start;
    padding: 0.4rem 0.8rem;
    border-radius: 0.5rem;
    border: none;
    background: #16a34a;
    color: white;
    font-weight: 600;
    cursor: pointer;
  }
  .primary:hover {
    opacity: 0.9;
  }
  .error {
    color: #b91c1c;
    font-size: 0.85rem;
  }
</style>
