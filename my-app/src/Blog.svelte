<script>
  let profileFilter = "";

  const profiles = [
    {
      name: "Jordan Miles",
      headline: "Aspiring Data Analyst · Community Advocate",
      location: "Cincinnati, OH",
      summary:
        "Focused on using data and storytelling to support other people impacted by wrongful conviction.",
      story:
        "Jordan spent 12 years wrongfully incarcerated. Since release, they have completed multiple data and digital literacy courses, volunteered with a local innocence organization, and helped mentor others navigating reentry.",
      skills: ["Excel", "SQL", "Python", "Public speaking"],
      interests: ["Data for justice", "Mentoring", "Community events"],
      lookingFor: "Entry-level data roles, research assistantships, or analyst internships."
    },
    {
      name: "Taylor Reed",
      headline: "Front-End Developer in Training",
      location: "Columbus, OH",
      summary:
        "Learning web development from the ground up and building projects that center accessibility and reentry.",
      story:
        "Taylor began coding through free online courses after release. They use personal projects to document their learning and to show employers how committed they are to continuous growth.",
      skills: ["HTML", "CSS", "JavaScript", "Svelte"],
      interests: ["Web design", "Accessibility", "Peer tutoring"],
      lookingFor: "Apprenticeships, junior front-end roles, or project-based collaborations."
    },
    {
      name: "Alex Rivera",
      headline: "Peer Navigator · Reentry Support",
      location: "Remote / Midwest",
      summary:
        "Supports others coming home by sharing resources, building networks, and organizing support groups.",
      story:
        "Alex lost 9 years to a wrongful conviction. They now coordinate virtual support circles, help others write resumes, and speak with employers about what real second chances look like.",
      skills: ["Facilitation", "Case navigation", "Writing", "Public speaking"],
      interests: ["Mental health", "Policy change", "Workshop facilitation"],
      lookingFor: "Roles with community organizations, advocacy groups, or reentry programs."
    }
  ];

  $: filteredProfiles = profiles.filter((person) => {
    if (!profileFilter) return true;
    const term = profileFilter.toLowerCase();
    return (
      person.name.toLowerCase().includes(term) ||
      person.location.toLowerCase().includes(term) ||
      person.headline.toLowerCase().includes(term) ||
      person.skills.some((s) => s.toLowerCase().includes(term)) ||
      person.interests.some((i) => i.toLowerCase().includes(term))
    );
  });
</script>

<section class="blog-layout">
  <!-- Left: main search + results -->
  <div class="blog-main card">
    <div class="blog-header">
      <h1>Search Profiles</h1>
      <p class="text-muted">
        Discover people rebuilding after wrongful conviction. Each profile is
        designed to show more than a resume: skills, interests, story, and what
        they are looking for next.
      </p>
      <input
        class="blog-search"
        type="text"
        placeholder="Search by name, skill, interest, or location…"
        bind:value={profileFilter}
      />
    </div>

    {#if filteredProfiles.length === 0}
      <p class="text-muted">No profiles match that search yet.</p>
    {:else}
      {#each filteredProfiles as person}
        <article class="post-card">
          <header class="post-header">
            <div>
              <h2 class="post-title">{person.name}</h2>
              <p class="post-meta">
                {person.headline} · {person.location}
              </p>
            </div>
          </header>

          <p class="post-summary">{person.summary}</p>

          <p class="post-body">
            <strong>Their story:</strong> {person.story}
          </p>

          <div class="tag-row">
            {#each person.skills as skill}
              <span class="tag-pill small">{skill}</span>
            {/each}
          </div>

          <p class="post-body" style="margin-top: 0.4rem;">
            <strong>Interests:</strong> {person.interests.join(", ")}
          </p>

          <p class="post-body" style="margin-top: 0.2rem;">
            <strong>Looking for:</strong> {person.lookingFor}
          </p>
        </article>
      {/each}
    {/if}
  </div>

  <!-- Right: explanation / guidance -->
  <aside class="blog-sidebar">
    <div class="card">
      <h2 class="side-heading">Why profiles matter</h2>
      <p class="text-muted">
        Traditional search results and background checks rarely tell the whole
        story. These profiles are meant to reimagine a person&apos;s digital
        footprint by highlighting their growth, skills, values, and goals.
      </p>
      <p class="text-muted">
        Employers, organizers, and community members can use this space to
        understand the person behind the case file and build more thoughtful
        connections.
      </p>
    </div>

    <div class="card mini">
      <p class="mini-title">Tips for exploring</p>
      <ul class="link-list">
        <li>Search by skills like “Excel,” “coding,” or “public speaking.”</li>
        <li>Search by interests such as “policy,” “design,” or “mentoring.”</li>
        <li>Search by location to find people near you.</li>
      </ul>
    </div>
  </aside>
</section>
