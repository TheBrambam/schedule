<script>
  // Courses is the same array used by "Your Classes"
  export let courses = [];

  const DAYS = ['Mon', 'Tue', 'Wed', 'Thu', 'Fri'];

  const START_HOUR = 8;
  const END_HOUR = 18; // 6pm

  const START_MIN = START_HOUR * 60;
  const END_MIN = END_HOUR * 60;
  const RANGE_MIN = END_MIN - START_MIN;

  const hours = Array.from(
    { length: END_HOUR - START_HOUR + 1 },
    (_, i) => START_HOUR + i
  );

  function timeToMinutes(t) {
    if (!t) return START_MIN;

    // Handle values like "09 : 00" by stripping spaces
    t = t.replace(/\s/g, '');
    const [h, m] = t.split(':').map((s) => Number(s.trim()));
    return h * 60 + m;
  }

  function coursesForDay(day, list = courses) {
    return list.filter((c) => {
      if (!c || !c.days) return false;

      const days = Array.isArray(c.days)
        ? c.days
        : String(c.days)
            .split(',')
            .map((d) => d.trim());

      return days.includes(day);
    });
  }

  function blockStyle(course) {
    const start = Math.max(START_MIN, timeToMinutes(course.startTime));
    const end = Math.min(END_MIN, timeToMinutes(course.endTime));
    const duration = Math.max(end - start, 15); // minimum height

    const top = ((start - START_MIN) / RANGE_MIN) * 100;
    const height = (duration / RANGE_MIN) * 100;

    return `top:${top}%;height:${height}%;`;
  }

  function formatHour(h) {
    const label = h % 12 === 0 ? 12 : h % 12;
    const ampm = h < 12 ? 'am' : 'pm';
    return `${label}${ampm}`;
  }

  // Rebuild a per-day map whenever `courses` changes
  let dayCourses = {};

  function buildDayCourses(list) {
    const map = {};
    for (const day of DAYS) {
      map[day] = coursesForDay(day, list);
    }
    return map;
  }

  // Line fix to build
  $: dayCourses = buildDayCourses(courses);
</script>

<div class="grid-container">
  <!-- Time labels on the left -->
  <div class="time-column">
    <div class="time-header"></div>
    {#each hours as h}
      <div class="time-label">
        {formatHour(h)}
      </div>
    {/each}
  </div>

  <!-- Days + schedule -->
  <div class="days-wrapper">
    <div class="days-header">
      {#each DAYS as d}
        <div class="day-header">{d}</div>
      {/each}
    </div>

    <div class="days-grid">
      {#each DAYS as day}
        <div class="day-column">
          <!-- background hour lines -->
          {#each hours as _}
            <div class="hour-slot"></div>
          {/each}

          <!-- one tall block per course that meets on this day -->
          {#each dayCourses[day] || [] as course (course.id)}
            <div class="class-block" style={blockStyle(course)}>
              <div class="class-name">{course.name}</div>
              <div class="class-meta">
                <span>{course.location || 'TBA'}</span>
                <span>{course.startTime}–{course.endTime}</span>
              </div>
            </div>
          {/each}
        </div>
      {/each}
    </div>
  </div>
</div>

<style>
  .grid-container {
    display: flex;
    border-radius: 0.75rem;
    border: 1px solid #ddd;
    overflow: hidden;
    background: #fafafa;
    font-size: 0.8rem;
  }

  .time-column {
    width: 3.6rem;
    border-right: 1px solid #ddd;
    background: #f3f4f6;
  }

  .time-header {
    height: 2rem;
    border-bottom: 1px solid #ddd;
  }

  .time-label {
    height: 2.4rem;
    display: flex;
    align-items: flex-start;
    justify-content: center;
    padding-top: 0.2rem;
    font-size: 0.75rem;
    color: #555;
  }

  .days-wrapper {
    flex: 1;
    display: flex;
    flex-direction: column;
  }

  .days-header {
    display: grid;
    grid-template-columns: repeat(5, 1fr);
    background: #f3f4f6;
    border-bottom: 1px solid #ddd;
  }

  .day-header {
    text-align: center;
    padding: 0.45rem 0;
    font-weight: 600;
    font-size: 0.8rem;
  }

  .days-grid {
    display: grid;
    grid-template-columns: repeat(5, 1fr);
  }

  .day-column {
    position: relative;
    border-left: 1px solid #eee;
    min-height: 480px;
  }

  .day-column:first-child {
    border-left: none;
  }

  .hour-slot {
    border-bottom: 1px dashed #eee;
    height: 2.4rem;
  }

  .class-block {
    position: absolute;
    left: 8%;
    right: 8%;
    border-radius: 0.5rem;
    padding: 0.25rem 0.4rem;
    background: rgba(37, 99, 235, 0.9);
    color: white;
    box-shadow: 0 2px 6px rgba(0, 0, 0, 0.2);
    overflow: hidden;
    display: flex;
    flex-direction: column;
    justify-content: center;
  }

  .class-name {
    font-weight: 600;
    font-size: 0.8rem;
    margin-bottom: 0.1rem;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }

  .class-meta {
    font-size: 0.7rem;
    display: flex;
    justify-content: space-between;
    gap: 0.25rem;
    flex-wrap: wrap;
  }
</style>
