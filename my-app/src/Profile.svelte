<script>
  let editMode = false;

  let name = "Jordan Miles";
  let initials = "JM";
  let title = "Aspiring Data Analyst · Community Advocate";
  let location = "Cincinnati, OH";

  let about = `
I am rebuilding my life through data, community,
and storytelling. This space is where I share who I am beyond a case file:
the skills I’m building, the people I support, and the future I am working toward.
  `.trim();

  let stats = {
    connections: 42,
    updates: 3
  };

  let skills = ["Excel", "SQL", "Python", "Public speaking"];

  // Bloom Updates = status updates + optional photo
  let bloomUpdates = [
    {
      id: 1,
      text:
        "Finished a spreadsheet project mapping local reentry resources. Next step: turning it into a simple dashboard.",
      imageUrl: "/images/jordan-1.png",
      timeAgo: "1 day ago",
      likes: 8,
      comments: 2
    },
    {
      id: 2,
      text:
        "Spoke with a student group about wrongful conviction and what real second chances look like.",
      imageUrl: "",
      timeAgo: "4 days ago",
      likes: 15,
      comments: 5
    },
    {
      id: 3,
      text:
        "Started a new data literacy course online. Grateful to be learning on my own terms.",
      imageUrl: "",
      timeAgo: "1 week ago",
      likes: 5,
      comments: 1
    }
  ];

  let newUpdateText = "";
  let newUpdateImageUrl = "";

  function toggleEdit() {
    editMode = !editMode;
  }

  function addBloomUpdate() {
	const text = newUpdateText.trim();
	if (!text) return;

	const newUpdate = {
		id: Date.now(),
		text,
		imageUrl: newUpdateImagePreview || "",  // Base64
		timeAgo: "Just now",
		likes: 0,
		comments: 0
	};

	bloomUpdates = [newUpdate, ...bloomUpdates];
	stats.updates++;

	// reset composer
	newUpdateText = "";
	newUpdateImageFile = null;
	newUpdateImagePreview = "";
}


  function viewAllBloomUpdates() {
    // future: navigate to a dedicated Bloom Updates / timeline subpage
    // for now, this is just a placeholder
    alert("Future: this will open a full Bloom Updates timeline page.");
  }

  let newUpdateImageFile = null;
  
  let newUpdateImagePreview = "";
  
  function handleImageUpload(event) {
	const file = event.target.files[0];
	if (file) {
		newUpdateImageFile = file;
		const reader = new FileReader();
		reader.onload = (e) => {
			newUpdateImagePreview = e.target.result;
	};
	reader.readAsDataURL(file);
  }
}
</script>

<section class="single-column">
  <div class="card profile-card">
    <div class="profile-banner"></div>
    <div class="profile-avatar">{initials}</div>

    <!-- header: identity + edit -->
    <div class="profile-header-row">
      <div class="profile-header-text">
        {#if editMode}
          <input
            class="profile-input profile-input-name"
            type="text"
            bind:value={name}
            placeholder="Name"
          />
          <input
            class="profile-input profile-input-title"
            type="text"
            bind:value={title}
            placeholder="Headline"
          />
          <input
            class="profile-input profile-input-location"
            type="text"
            bind:value={location}
            placeholder="Location"
          />
        {:else}
          <h2 class="profile-name">{name}</h2>
          <p class="profile-title">{title}</p>
          <p class="profile-location">{location}</p>
        {/if}
      </div>

      <button class="outline-btn small-btn" type="button" on:click={toggleEdit}>
        {editMode ? "Done" : "Edit profile"}
      </button>
    </div>

    <!-- stats row -->
    <div class="profile-stats-row">
      <div>
        <strong>{stats.connections}</strong>
        <span>connections</span>
      </div>
      <div>
        <strong>{stats.updates}</strong>
        <span>Bloom updates</span>
      </div>
    </div>

    <hr />

    <!-- About -->
    <p class="profile-section-title">About</p>
    {#if editMode}
      <textarea
        class="profile-textarea"
        rows="4"
        bind:value={about}
      />
    {:else}
      <p class="profile-about">{about}</p>
    {/if}

    <!-- Skills -->
    <p class="profile-section-title">Skills</p>
    <div class="tag-row">
      {#each skills as skill}
        <span class="tag-pill small">{skill}</span>
      {/each}
    </div>

    <!-- Bloom Updates composer -->
    <p class="profile-section-title">Bloom updates</p>
    <div class="composer-card">
		<textarea
			class="composer-input"
			rows="3"
			placeholder="Share a win, reflection, or moment…"
			bind:value={newUpdateText}
		/>

		<!-- IMAGE UPLOAD -->
		<label class="img-upload-label">
			<input
			type="file"
			accept="image/*"
			on:change={handleImageUpload}
			class="img-upload-input"
			/>
			📷 Add a photo
		</label>

		<!-- PREVIEW -->
		{#if newUpdateImagePreview}
			<div class="composer-preview">
			<img src={newUpdateImagePreview} alt="Preview" />
			</div>
		{/if}

		<div class="composer-actions">
			<button
			class="primary-btn small-btn"
			type="button"
			on:click={addBloomUpdate}
			>
			Post Bloom Update
			</button>
		</div>
	</div>


    <!-- Bloom Updates feed -->
    <div class="bloom-feed-header">
      <span class="bloom-feed-title">Recent Bloom Updates</span>
      <button
        class="inline-link bloom-view-all"
        type="button"
        on:click={viewAllBloomUpdates}
      >
        View all
      </button>
    </div>

    <div class="bloom-feed">
      {#if bloomUpdates.length === 0}
        <p class="profile-about">
          You haven’t posted any Bloom Updates yet.
          Use this space to reclaim your digital footprint with your own words and images.
        </p>
      {:else}
        {#each bloomUpdates as post}
          <article class="bloom-card">
            <header class="profile-post-header">
              <div class="profile-post-avatar">{initials}</div>
              <div>
                <p class="profile-post-name">{name}</p>
                <p class="profile-post-meta">
                  {title} · <span>{post.timeAgo}</span>
                </p>
              </div>
            </header>

            <p class="profile-post-text">{post.text}</p>

            {#if post.imageUrl}
              <div class="profile-post-image">
                <img src={post.imageUrl} alt="Bloom update image" />
              </div>
            {/if}

            <div class="profile-post-footer">
              <span>❤️ {post.likes}</span>
              <span>💬 {post.comments}</span>
            </div>
          </article>
        {/each}
      {/if}
    </div>

    <!-- Connect -->
    <p class="profile-section-title">Connect</p>
    <p class="profile-about" style="margin-bottom: 1.1rem;">
      I’m open to fair-chance roles, collaborations, and speaking with employers,
      organizers, or students about wrongful conviction and reentry.
      <br /><br />
      <em>(Placeholder for email / contact link here.)</em>
    </p>
  </div>
</section>

<style>
  /* layout + alignment */

  .profile-header-row {
    display: flex;
    align-items: flex-start;
    justify-content: space-between;
    gap: 0.75rem;
    padding: 0.75rem 1.25rem 0.3rem;
  }

  .profile-header-text {
    flex: 1;
  }

  .profile-stats-row {
    display: flex;
    justify-content: flex-start;
    gap: 1.5rem;
    padding: 0 1.25rem 0.4rem;
    font-size: 0.8rem;
  }

  .profile-stats-row div {
    display: flex;
    flex-direction: column;
    gap: 0.05rem;
  }

  .profile-stats-row strong {
    font-size: 0.95rem;
  }

  .profile-stats-row span {
    color: var(--text-muted);
  }

  .profile-input {
    width: 100%;
    padding: 0.35rem 0.6rem;
    border-radius: 6px;
    border: 1px solid var(--border-subtle);
    font-size: 0.9rem;
    margin-bottom: 0.3rem;
  }

  .profile-input-name {
    font-weight: 600;
  }

  .profile-textarea {
    width: calc(100% - 2.5rem);
    margin: 0.25rem 1.25rem 0.7rem;
    padding: 0.45rem 0.6rem;
    border-radius: 8px;
    border: 1px solid var(--border-subtle);
    font-size: 0.9rem;
    resize: vertical;
    min-height: 80px;
  }

  /* composer */

  .composer-card {
    margin: 0.25rem 1.25rem 0.8rem;
    padding: 0.6rem 0.7rem;
    border-radius: 10px;
    background: #fff7f8;
    border: 1px solid #f0d7df;
  }

  .composer-input {
    width: 100%;
    border-radius: 8px;
    border: 1px solid var(--border-subtle);
    padding: 0.45rem 0.55rem;
    font-size: 0.9rem;
    resize: vertical;
    margin-bottom: 0.4rem;
  }

  .composer-actions {
    display: flex;
    justify-content: flex-end;
    margin-top: 0.2rem;
  }

  /* Bloom Updates feed */

  .bloom-feed-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin: 0.1rem 1.25rem 0.3rem;
  }

  .bloom-feed-title {
    font-size: 0.85rem;
    font-weight: 600;
    color: var(--lotus-green);
    text-transform: uppercase;
    letter-spacing: 0.08em;
  }

  .bloom-view-all {
    font-size: 0.75rem;
  }

  .bloom-feed {
    margin: 0.2rem 1.25rem 0.9rem;
  }

  .bloom-card {
    border-radius: 10px;
    border: 1px solid #f0d7df;
    padding: 0.65rem 0.7rem;
    background: #ffffff;
    margin-bottom: 0.6rem;
  }

  .profile-post-header {
    display: flex;
    gap: 0.5rem;
    align-items: center;
    margin-bottom: 0.25rem;
  }

  .profile-post-avatar {
    width: 32px;
    height: 32px;
    border-radius: 50%;
    background: var(--lotus-green);
    color: #fff;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 0.8rem;
    font-weight: 600;
  }

  .profile-post-name {
    font-size: 0.9rem;
    margin: 0;
  }

  .profile-post-meta {
    font-size: 0.75rem;
    margin: 0.05rem 0 0;
    color: var(--text-muted);
  }

  .profile-post-text {
    font-size: 0.9rem;
    margin: 0.35rem 0 0.35rem;
  }

  .profile-post-image img {
    width: 100%;
    max-height: 220px;
    border-radius: 8px;
    object-fit: cover;
  }

  .profile-post-footer {
    margin-top: 0.3rem;
    font-size: 0.78rem;
    color: var(--text-muted);
    display: flex;
    gap: 1rem;
  }

  /* alignment fixes */

  .profile-card p,
  .profile-section-title,
  .profile-about,
  .link-list,
  .tag-row,
  .bloom-feed {
    text-align: left;
  }

  .profile-section-title {
    margin-left: 1.25rem;
  }

  .profile-about {
    margin-left: 1.25rem;
    margin-right: 1.25rem;
  }

  .tag-row {
    margin-left: 1.25rem;
    margin-right: 1.25rem;
  }

  ul.link-list {
    padding-left: 1.25rem;
    margin: 0.4rem 1.25rem 0.8rem;
  }

  .img-upload-label {
  display: inline-block;
  font-size: 0.8rem;
  color: var(--lotus-green);
  cursor: pointer;
  margin-bottom: 0.4rem;
}

.img-upload-input {
  display: none;
}

.composer-preview {
  margin-top: 0.4rem;
  margin-bottom: 0.4rem;
}

.composer-preview img {
  width: 100%;
  max-height: 220px;
  border-radius: 8px;
  object-fit: cover;
  border: 1px solid rgba(55,75,30,0.2);
}

</style>
