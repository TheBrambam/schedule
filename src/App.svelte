<script>
  import CourseForm from './components/CourseForm.svelte';
  import CourseList from './components/CourseList.svelte';
  import ScheduleGrid from './components/ScheduleGrid.svelte';

  // Catalog for dropdown
  const COURSE_CATALOG = [
    {
      key: 'CHEM1040',
      name: 'General Chemistry I',
      location: 'Old Chem',
      defaultDays: ['Mon', 'Wed', 'Fri'],
      defaultStart: '09:00',
      defaultEnd: '10:15',
      year: 'First',
      semester: 'Fall'
    },
		
    {
      key: 'CHEM1040L',
      name: 'General Chemistry Laboratory I',
      location: 'Chem Lab',
      defaultDays: ['Wed'],
      defaultStart: '11:00',
      defaultEnd: '12:15',
      year: 'First',
      semester: 'Fall'
    },
		
    {
      key: 'CS1100',
      name: 'Introduction to Computer Science',
      location: 'One of the Linder buildings',
      defaultDays: ['Tue', 'Thu'],
      defaultStart: '11:00',
      defaultEnd: '12:15',
      year: 'First',
      semester: 'Fall'
    },
		
    {
      key: 'ENED1100',
      name: 'Foundations of Engineering Design Thinking I',
      location: 'Rhodes 800',
      defaultDays: ['Mon', 'Wed', 'Fri'],
      defaultStart: '13:00',
      defaultEnd: '13:50',
      year: 'First',
      semester: 'Fall'
    },
		
    {
      key: 'ENGL1001',
      name: 'English Composition',
      location: 'Library',
      defaultDays: ['Wed'],
      defaultStart: '14:00',
      defaultEnd: '15:15',
      year: 'First',
      semester: 'Fall'
    },
		
    {
      key: 'MATH1061',
      name: 'Calculus I',
      location: 'French',
      defaultDays: ['Wed'],
      defaultStart: '14:00',
      defaultEnd: '15:15',
      year: 'First',
      semester: 'Fall'
    },
		
    {
      key: 'CS1021C',
      name: 'Computer Science I',
      location: 'Teachers Dyer',
      defaultDays: ['Wed'],
      defaultStart: '14:00',
      defaultEnd: '15:15',
      year: 'First',
      semester: 'Spring'
    },
		
    {
      key: 'ENED1120',
      name: 'Foundations of Engineering Design Thinking II',
      location: 'Rhodes',
      defaultDays: ['Wed'],
      defaultStart: '14:00',
      defaultEnd: '15:15',
      year: 'First',
      semester: 'Spring'
    },
		
    {
      key: 'MATH1062',
      name: 'Calculus II',
      location: 'French',
      defaultDays: ['Wed'],
      defaultStart: '14:00',
      defaultEnd: '15:15',
      year: 'First',
      semester: 'Spring'
    },
		
    {
      key: 'PD1011',
      name: 'Introduction to COOP for CEAS',
      location: 'Online',
      defaultDays: ['Mon'],
      defaultStart: '14:00',
      defaultEnd: '15:15',
      year: 'First',
      semester: 'Spring'
    },
		
    {
      key: 'CS2011',
      name: 'Introduction to Computer Systems',
      location: 'Baldwin',
      defaultDays: ['Wed'],
      defaultStart: '14:00',
      defaultEnd: '15:15',
      year: 'Second',
      semester: 'Spring'
    },
		
    {
      key: 'CS2021',
      name: 'Python Programming',
      location: 'RecCen',
      defaultDays: ['Wed'],
      defaultStart: '14:00',
      defaultEnd: '15:15',
      year: 'Second',
      semester: 'Spring'
    },
		
    {
      key: 'CS2028C',
      name: 'Data Structures',
      location: 'Teachers Dyer',
      defaultDays: ['Wed'],
      defaultStart: '14:00',
      defaultEnd: '15:15',
      year: 'Second',
      semester: 'Spring'
    },
		
    {
      key: 'IT2030C',
      name: 'Information Security and Assurance',
      location: 'Online',
      defaultDays: ['Fri'],
      defaultStart: '14:00',
      defaultEnd: '15:15',
      year: 'Second',
      semester: 'Spring'
    },
		
    {
      key: 'MATH2076',
      name: 'Linear Algebra',
      location: 'French',
      defaultDays: ['Wed'],
      defaultStart: '14:00',
      defaultEnd: '15:15',
      year: 'Second',
      semester: 'Spring'
    },
		
    {
      key: 'CS2071',
      name: 'Discrete Computational Structures',
      location: 'Teachers Dyer',
      defaultDays: ['Wed'],
      defaultStart: '14:00',
      defaultEnd: '15:15',
      year: 'Third',
      semester: 'Fall'
    },
		
    {
      key: 'CS3003',
      name: 'Programming Languages',
      location: 'Teachers Dyer',
      defaultDays: ['Wed'],
      defaultStart: '14:00',
      defaultEnd: '15:15',
      year: 'Third',
      semester: 'Fall'
    },
		
    {
      key: 'ENGL2089',
      name: 'Intermediate Composition',
      location: 'Online',
      defaultDays: ['Wed'],
      defaultStart: '14:00',
      defaultEnd: '15:15',
      year: 'Third',
      semester: 'Fall'
    },
		
    {
      key: 'STAT2037',
      name: 'Probability and Statistics I',
      location: 'Online',
      defaultDays: ['Wed'],
      defaultStart: '14:00',
      defaultEnd: '15:15',
      year: 'Third',
      semester: 'Fall'
    },
		
    {
      key: 'COMM1071',
      name: 'Introduction to Effective Speaking',
      location: 'Library',
      defaultDays: ['Wed'],
      defaultStart: '14:00',
      defaultEnd: '15:15',
      year: 'Third',
      semester: 'Summer'
    },
		
    {
      key: 'CS4071',
      name: 'Design and Analysis of Algorithms',
      location: 'Rhodes',
      defaultDays: ['Wed'],
      defaultStart: '14:00',
      defaultEnd: '15:15',
      year: 'Third',
      semester: 'Summer'
    },
		
    {
      key: 'CS4092',
      name: 'Database Design an dDevelopment',
      location: 'Teachers Dyer',
      defaultDays: ['Wed'],
      defaultStart: '14:00',
      defaultEnd: '15:15',
      year: 'Third',
      semester: 'Summer'
    },
		
    {
      key: 'EECE3093C',
      name: 'Software Engineering',
      location: 'Baldwin',
      defaultDays: ['Wed'],
      defaultStart: '14:00',
      defaultEnd: '15:15',
      year: 'Third',
      semester: 'Summer'
    },
		
    {
      key: 'CS4033',
      name: 'Artificial Intelligence: Principles and Applications',
      location: 'DAAP',
      defaultDays: ['Wed'],
      defaultStart: '14:00',
      defaultEnd: '15:15',
      year: 'Fourth',
      semester: 'Spring'
    },
		
    {
      key: 'CS4065',
      name: 'Computer Networks and Networked Computing',
      location: 'Swift',
      defaultDays: ['Wed'],
      defaultStart: '14:00',
      defaultEnd: '15:15',
      year: 'Fourth',
      semester: 'Spring'
    },
		
    {
      key: 'EECE4029',
      name: 'Operating Systems & Systems Programming',
      location: 'Rhodes',
      defaultDays: ['Wed'],
      defaultStart: '14:00',
      defaultEnd: '15:15',
      year: 'Fourth',
      semester: 'Spring'
    },
		
    {
      key: 'ENGL4092',
      name: 'Technical and Scientific Writing',
      location: 'Online',
      defaultDays: ['Wed'],
      defaultStart: '14:00',
      defaultEnd: '15:15',
      year: 'Fourth',
      semester: 'Spring'
    },
    {
      key: 'CS5001',
      name: 'Computer Science Senior Design I',
      location: 'Teachers Dyer',
      defaultDays: ['Wed'],
      defaultStart: '14:00',
      defaultEnd: '15:15',
      year: 'Fifth',
      semester: 'Fall'
    },
		
    {
      key: 'CS5170',
      name: 'The Theory of Formal Languages and Automata',
      location: 'Teachers Dyer',
      defaultDays: ['Wed'],
      defaultStart: '14:00',
      defaultEnd: '15:15',
      year: 'Fifth',
      semester: 'Fifth'
    },
		
    {
      key: 'CS5002',
      name: 'Computer Science Senior Design II',
      location: 'Teachers Dyer',
      defaultDays: ['Wed'],
      defaultStart: '14:00',
      defaultEnd: '15:15',
      year: 'Fifth',
      semester: 'Spring'
    },
		
    {
      key: 'MATH2063',
      name: 'Multivariable Calculus',
      location: 'French',
      defaultDays: ['Wed'],
      defaultStart: '14:00',
      defaultEnd: '15:15'
    },
		
    {
      key: 'MATH2073',
      name: 'Ordinary Differential Equations',
      location: 'Teachers Dyer',
      defaultDays: ['Wed'],
      defaultStart: '14:00',
      defaultEnd: '15:15'
    },
		
    {
      key: 'MATH4012',
      name: 'Differential Geometry and Topology',
      location: 'French',
      defaultDays: ['Wed'],
      defaultStart: '14:00',
      defaultEnd: '15:15'
    },
		
    {
      key: 'MATH5107',
      name: 'Partial Differential Equations and Fourier Analysis',
      location: 'French',
      defaultDays: ['Wed'],
      defaultStart: '14:00',
      defaultEnd: '15:15'
    },
		
    {
      key: 'MATH5101',
      name: 'Advanced Calculus I',
      location: 'French',
      defaultDays: ['Wed'],
      defaultStart: '14:00',
      defaultEnd: '15:15'
    },
		
    {
      key: 'MATH5102',
      name: 'Advanced Calculus II',
      location: 'French',
      defaultDays: ['Wed'],
      defaultStart: '14:00',
      defaultEnd: '15:15'
    },
		
    {
      key: 'MATH3002',
      name: 'Introduction to Analysis',
      location: 'French',
      defaultDays: ['Wed'],
      defaultStart: '14:00',
      defaultEnd: '15:15'
    },
		
    {
      key: 'MATH5105',
      name: 'Introduction to Complex Analysis',
      location: 'French',
      defaultDays: ['Wed'],
      defaultStart: '14:00',
      defaultEnd: '15:15'
    }
  ];

  let courses = [];
  let nextId = 1;

  function addCourse(course) {
    const newCourse = { ...course, id: nextId++ };
    courses = [...courses, newCourse];
  }

  function deleteCourse(id) {
    courses = courses.filter((c) => c.id !== id);
  }

  function detectConflicts(courses) {
    const conflicts = [];
    const byDay = {};

    for (const c of courses) {
      for (const day of c.days) {
        if (!byDay[day]) byDay[day] = [];
        byDay[day].push(c);
      }
    }

    function overlaps(a, b) {
      return a.startTime < b.endTime && b.startTime < a.endTime;
    }

    for (const day in byDay) {
      const list = byDay[day];
      for (let i = 0; i < list.length; i++) {
        for (let j = i + 1; j < list.length; j++) {
          if (overlaps(list[i], list[j])) {
            conflicts.push(
              `${day}: "${list[i].name}" overlaps with "${list[j].name}"`
            );
          }
        }
      }
    }
    return conflicts;
  }

  $: conflicts = detectConflicts(courses);
</script>

<main class="app">
  <header class="app-header">
    <h1>College Class Scheduler</h1>
    <p>Create your weekly class schedule and check for time conflicts.</p>
  </header>

  <section class="app-layout">
    <div class="left-panel">
      <section class="panel">
        <h2>Add Class</h2>
        <CourseForm
          availableCourses={COURSE_CATALOG}
          on:addCourse={(e) => addCourse(e.detail)}
        />
      </section>

      <section class="panel">
        <h2>Your Classes</h2>
        <CourseList {courses} on:deleteCourse={(e) => deleteCourse(e.detail)} />
      </section>

      {#if conflicts.length > 0}
        <section class="panel panel-warning">
          <h2>Conflicts Detected</h2>
          <ul>
            {#each conflicts as c}
              <li>{c}</li>
            {/each}
          </ul>
        </section>
      {/if}
    </div>

    <div class="right-panel">
      <section class="panel">
        <h2>Weekly Schedule</h2>
        <!-- Same courses array that drives the list -->
        <ScheduleGrid {courses} />
      </section>
    </div>
  </section>
</main>
