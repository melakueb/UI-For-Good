<script>
  const courses = [
    {
      name: "Digital Literacy Basics",
      provider: "Reentry Learning Hub",
      length: "3 hours"
    },
    {
      name: "Intro to Spreadsheets",
      provider: "Open Skills",
      length: "2 hours"
    },
    {
      name: "First Steps in HTML & CSS",
      provider: "Open Web Skills",
      length: "4 hours"
    }
  ];
</script>

<section class="single-column">
  <div class="card">
    <h1>Upskilling Hub</h1>
    <p class="text-muted">
      Short, focused learning paths to help you rebuild skills and earn
      credentials you can add to your profile and blog.
    </p>

    {#each courses as course}
      <div class="course-card">
        <p class="course-name">{course.name}</p>
        <p class="course-provider">{course.provider}</p>
        <p class="course-length">{course.length}</p>
        <button class="outline-btn tiny-btn" type="button">
          Start
        </button>
      </div>
    {/each}
  </div>
</section>
