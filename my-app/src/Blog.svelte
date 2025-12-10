<script>
  import { onMount } from "svelte";

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
      lookingFor:
        "Entry-level data roles, research assistantships, or analyst internships.",
      bloomUpdates: [
        {
          id: 1,
          text:
            "Finished a spreadsheet project mapping local reentry resources. Next step: turning it into a simple dashboard.",
          imageUrl: "/images/jordan-1.png",
          timeAgo: "1 day ago"
        },
        {
          id: 2,
          text:
            "Started a new data literacy course online. Grateful to be learning on my own terms.",
          imageUrl: "",
          timeAgo: "1 week ago"
        }
      ]
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
      lookingFor:
        "Apprenticeships, junior front-end roles, or project-based collaborations.",
      bloomUpdates: [
        {
          id: 1,
          text:
            "Deployed my first simple website sharing my story and linking to Lotus. It feels powerful to shape my own digital footprint.",
          imageUrl: "/images/taylor-1.png",
          timeAgo: "5 days ago"
        }
      ]
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
      lookingFor:
        "Roles with community organizations, advocacy groups, or reentry programs.",
      bloomUpdates: [
        {
          id: 1,
          text:
            "Hosted a virtual support circle this week. Hearing everyone’s wins, big and small, keeps me going.",
          imageUrl: "",
          timeAgo: "1 week ago"
        }
      ]
    }
  ];

  // Filter profiles
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

  // Derive latest Bloom update per profile (assumes last item is most recent)
  $: enhancedProfiles = filteredProfiles.map((person) => {
    const updates = person.bloomUpdates || [];
    const latestBloom =
      updates.length > 0 ? updates[updates.length - 1] : null;
    return { ...person, latestBloom };
  });

  // ---------- Affirmations from CSV ----------

  let affirmations = [];
  let currentAffirmationIndex = 0;
  let loadingAffirmations = true;
  let affirmationError = "";

  onMount(async () => {
    try {
      const res = await fetch("positive_affirmation.csv");
      if (!res.ok) {
        throw new Error("Failed to load affirmations");
      }

      const text = await res.text();
      const lines = text
        .split("\n")
        .map((l) => l.trim())
        .filter((l) => l.length > 0);

      // Remove header
      const dataLines = lines.slice(1);

      affirmations = dataLines
        .map((line) => {
          // basic CSV split by comma
          const [rawAffirmation, rawTag] = line.split(",");
          if (!rawAffirmation) return null;

          const affirmation = rawAffirmation.replace(/^"|"$/g, "").trim();
          const tag = (rawTag || "").replace(/^"|"$/g, "").trim();

          return { affirmation, tag };
        })
        .filter(Boolean);

      loadingAffirmations = false;
    } catch (err) {
      console.error(err);
      affirmationError = "Could not load affirmations right now.";
      loadingAffirmations = false;
    }
  });

  $: currentAffirmation =
    affirmations.length > 0
      ? affirmations[currentAffirmationIndex % affirmations.length]
      : null;

  function nextAffirmation() {
    if (affirmations.length === 0) return;
    currentAffirmationIndex =
      (currentAffirmationIndex + 1) % affirmations.length;
  }
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

    {#if enhancedProfiles.length === 0}
      <p class="text-muted">No profiles match that search yet.</p>
    {:else}
      {#each enhancedProfiles as person (person.name)}
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

          {#if person.latestBloom}
            <div class="mini-post">
              <div class="mini-post-header">
                <span class="mini-post-name">Latest Bloom update</span>
                <span class="mini-post-time">{person.latestBloom.timeAgo}</span>
              </div>
              <p class="mini-post-text">{person.latestBloom.text}</p>

              {#if person.latestBloom.imageUrl}
                <div class="mini-post-image">
                  <img
                    src={person.latestBloom.imageUrl}
                    alt={"Latest Bloom update from " + person.name}
                  />
                </div>
              {/if}
            </div>
          {/if}
        </article>
      {/each}
    {/if}
  </div>

  <!-- Right: explanation / guidance + Affirmations -->
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

    <!-- NEW: Affirmations card -->
    <div class="card affirmation-card">
      <h2 class="side-heading">Lotus Affirmation</h2>

      {#if loadingAffirmations}
        <p class="text-muted" style="font-size: 0.85rem;">
          Loading affirmations…
        </p>
      {:else if affirmationError}
        <p class="text-muted" style="font-size: 0.85rem;">
          {affirmationError}
        </p>
      {:else if currentAffirmation}
        <p class="affirmation-text">
          “{currentAffirmation.affirmation}”
        </p>

        {#if currentAffirmation.tag}
          <p class="affirmation-tag">#{currentAffirmation.tag}</p>
        {/if}

        <button
          type="button"
          class="outline-btn small-btn affirmation-next-btn"
          on:click={nextAffirmation}
        >
          Another affirmation
        </button>
      {:else}
        <p class="text-muted" style="font-size: 0.85rem;">
          No affirmations available yet.
        </p>
      {/if}
    </div>
  </aside>
</section>

<style>
  .mini-post {
    margin-top: 0.7rem;
    padding: 0.55rem 0.6rem;
    border-radius: 10px;
    background: #fff7f8;
    border: 1px solid #f0d7df;
  }

  .mini-post-header {
    display: flex;
    justify-content: space-between;
    font-size: 0.8rem;
    margin-bottom: 0.2rem;
  }

  .mini-post-name {
    font-weight: 600;
    color: var(--lotus-green);
  }

  .mini-post-time {
    color: var(--text-muted);
  }

  .mini-post-text {
    font-size: 0.88rem;
    margin: 0.1rem 0 0.35rem;
  }

  .mini-post-image img {
    width: 100%;
    max-height: 180px;
    border-radius: 8px;
    object-fit: cover;
  }

  /* Affirmations card */

  .affirmation-card {
    background: #fff7f8;
    border: 1px solid var(--border-subtle);
  }

  .affirmation-text {
    font-size: 0.95rem;
    line-height: 1.6;
    margin: 0.5rem 0 0.4rem;
  }

  .affirmation-tag {
    font-size: 0.8rem;
    color: var(--lotus-green);
    font-weight: 600;
    margin-bottom: 0.4rem;
  }

  .affirmation-next-btn {
    margin-top: 0.1rem;
  }
</style>
