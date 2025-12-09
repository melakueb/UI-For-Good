<script>
  const jobs = [
    {
      company: "FairChance Tech",
      title: "Junior Support Specialist",
      location: "Remote",
      tags: ["Entry-Level", "IT Support"]
    },
    {
      company: "Community Impact Lab",
      title: "Program Assistant",
      location: "Cincinnati, OH",
      tags: ["Non-Profit", "Part-Time"]
    }
  ];
</script>

<section class="single-column">
  <div class="card">
    <h1>Job Board</h1>
    <p class="text-muted">
      Roles from organizations that are open to fair-chance hiring and
      understand wrongful conviction.
    </p>

    {#each jobs as job}
      <article class="job-card">
        <h2 class="job-title">{job.title}</h2>
        <p class="job-company">{job.company}</p>
        <p class="job-location">{job.location}</p>
        <div class="tag-row">
          {#each job.tags as tag}
            <span class="tag-pill small">{tag}</span>
          {/each}
        </div>
        <button class="outline-btn small-btn" type="button">
          View details
        </button>
      </article>
    {/each}
  </div>
</section>
