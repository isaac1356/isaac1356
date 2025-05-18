<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Nighters Movies - Watch & Manage</title>
<style>
  /* Reset */
  *, *::before, *::after {
    box-sizing: border-box;
  }
  body {
    margin: 0; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background: #10121a; color: #e0e6f1;
    min-height: 100vh;
    display: flex; flex-direction: column;
  }
  a {
    color: #61dafb;
    text-decoration: none;
  }
  a:hover {
    text-decoration: underline;
  }
  header {
    background: #141822;
    padding: 10px 20px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    border-bottom: 2px solid #61dafb;
  }
  header h1 {
    margin: 0;
    font-weight: 700;
    font-size: 1.8rem;
    color: #61dafb;
    cursor: default;
  }
  nav button {
    background: none;
    border: 2px solid #61dafb;
    color: #61dafb;
    padding: 6px 15px;
    margin-left: 10px;
    border-radius: 6px;
    font-weight: 600;
    cursor: pointer;
    transition: background-color 0.25s ease, color 0.25s ease;
  }
  nav button:hover {
    background-color: #61dafb;
    color: #10121a;
  }
  main {
    flex-grow: 1;
    padding: 20px;
    overflow-y: auto;
  }
  .hidden {
    display: none !important;
  }
  /* User and Admin container wrapper */
  #app {
    max-width: 1200px;
    margin: 0 auto;
  }
  /* Categories Nav */
  #categories {
    display: flex;
    flex-wrap: wrap;
    gap: 12px;
    margin: 10px 0 20px;
  }
  #categories button {
    padding: 8px 16px;
    background: #222832;
    border: none;
    border-radius: 24px;
    color: #61dafb;
    font-weight: 600;
    cursor: pointer;
    user-select: none;
    transition: background-color 0.3s ease;
  }
  #categories button.active,
  #categories button:hover {
    background: #61dafb;
    color: #10121a;
  }
  /* Movie List */
  #movie-list {
    display: grid;
    grid-template-columns: repeat(auto-fill,minmax(220px,1fr));
    grid-gap: 18px;
  }
  .movie-card {
    background: #222832;
    border-radius: 12px;
    overflow: hidden;
    box-shadow: 0 2px 8px rgb(0 0 0 / 0.7);
    display: flex;
    flex-direction: column;
    transition: transform 0.3s ease;
  }
  .movie-card:hover {
    transform: translateY(-6px);
  }
  .movie-poster {
    width: 100%;
    height: 300px;
    object-fit: cover;
    background: #171c29;
  }
  .movie-content {
    padding: 12px 15px;
    flex-grow: 1;
    display: flex; flex-direction: column;
  }
  .movie-title {
    margin: 0 0 6px;
    font-weight: 700;
    font-size: 1.1rem;
    color: #61dafb;
  }
  .movie-desc {
    flex-grow: 1;
    font-size: 0.9rem;
    color: #cbd5e0;
    margin-bottom: 10px;
  }
  .movie-genre {
    font-size: 0.8rem;
    font-weight: 600;
    color: #7f8fa6;
  }
  .movie-rating {
    margin-top: 6px;
  }
  .star {
    color: gold;
    font-size: 1rem;
  }
  /* Video Player */
  #video-player-container {
    margin-top: 30px;
    background: #222832;
    border-radius: 12px;
    padding: 20px;
  }
  #video-player {
    width: 100%;
    max-height: 480px;
    border-radius: 10px;
    background: #000;
    outline: none;
  }
  #video-details {
    margin-top: 12px;
    color: #a3b1cc;
  }
  /* Review section */
  #reviews {
    margin-top: 24px;
    background: #1c2231;
    border-radius: 10px;
    padding: 16px 20px;
  }
  #reviews h3 {
    margin-top: 0;
    color: #61dafb;
  }
  #review-list {
    max-height: 200px;
    overflow-y: auto;
    margin: 12px 0;
    border-top: 1px solid #2a3450;
    border-bottom: 1px solid #2a3450;
    padding: 8px 0;
  }
  .review-item {
    border-bottom: 1px solid #2a3450;
    padding: 6px 0;
    font-size: 0.9rem;
  }
  .review-user {
    font-weight: 700;
    color: #80b3ff;
  }
  .review-text {
    margin: 4px 0 0;
  }
  .review-rating {
    color: gold;
    font-size: 0.9rem;
  }
  #add-review {
    display: flex;
    flex-wrap: wrap;
    align-items: center;
    gap: 10px;
  }
  #review-text {
    flex-grow: 1;
    padding: 8px;
    border-radius: 6px;
    border: none;
    font-size: 1rem;
  }
  #review-rating {
    width: 60px;
    font-size: 1rem;
    padding: 8px;
    border-radius: 6px;
    border: none;
  }
  #submit-review {
    background: #61dafb;
    border: none;
    color: #10121a;
    font-weight: 700;
    cursor: pointer;
    padding: 8px 16px;
    border-radius: 6px;
    transition: background-color 0.3s ease;
  }
  #submit-review:hover {
    background: #4db0f7;
  }
  /* Login/Register */
  #auth-area {
    max-width: 400px;
    margin: 40px auto;
    background: #222832;
    padding: 24px 32px;
    border-radius: 12px;
    box-shadow: 0 2px 8px rgba(0,0,0,0.7);
  }
  #auth-area h2 {
    color: #61dafb;
    text-align: center;
    margin-bottom: 20px;
    font-weight: 700;
  }
  #auth-area form {
    display: flex;
    flex-direction: column;
  }
  #auth-area label {
    margin-bottom: 6px;
    font-weight: 600;
  }
  #auth-area input[type="email"],
  #auth-area input[type="password"] {
    padding: 10px 12px;
    margin-bottom: 18px;
    border: none;
    border-radius: 6px;
    font-size: 1rem;
  }
  #auth-area button {
    background: #61dafb;
    border: none;
    cursor: pointer;
    color: #10121a;
    font-weight: 700;
    padding: 10px;
    border-radius: 6px;
    transition: background-color 0.3s ease;
  }
  #auth-area button:hover {
    background: #4db0f7;
  }
  #auth-error {
    color: #ff7675;
    font-weight: 700;
    margin-bottom: 12px;
    text-align: center;
  }
  /* Admin Panel */
  #admin-panel {
    max-width: 1000px;
    margin: 20px auto 50px;
    background: #222832;
    padding: 20px 30px;
    border-radius: 12px;
    box-shadow: 0 2px 8px rgb(0 0 0 / 0.7);
    color: #a3b1cc;
  }
  #admin-panel h2 {
    color: #61dafb;
    margin-top: 0;
    margin-bottom: 24px;
    font-weight: 700;
  }
  #content-management, #user-management {
    margin-bottom: 32px;
  }
  #content-management h3, #user-management h3 {
    margin-top: 0;
    margin-bottom: 16px;
    color: #61dafb;
    text-transform: uppercase;
    letter-spacing: 1.2px;
    font-weight: 600;
  }
  #admin-movie-list {
    max-height: 300px;
    overflow-y: auto;
    border-top: 1px solid #2a3450;
    border-bottom: 1px solid #2a3450;
  }
  .admin-movie-item {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 8px;
    border-bottom: 1px solid #2a3450;
  }
  .admin-movie-details {
    flex-grow: 1;
    margin-left: 10px;
  }
  .admin-button {
    background: #61dafb;
    border: none;
    color: #10121a;
    padding: 6px 12px;
    margin-left: 10px;
    border-radius: 6px;
    cursor: pointer;
    font-weight: 600;
    transition: background-color 0.2s ease;
  }
  .admin-button:hover {
    background: #4db0f7;
  }
  #admin-add-form {
    margin-top: 20px;
  }
  #admin-add-form input, #admin-add-form select, #admin-add-form textarea {
    width: 100%;
    margin-bottom: 12px;
    padding: 10px;
    border-radius: 6px;
    border: none;
    font-size: 1rem;
  }
  #admin-add-form button {
    width: 100%;
    background: #61dafb;
    border: none;
    padding: 12px;
    border-radius: 8px;
    font-weight: 700;
    color: #10121a;
    cursor: pointer;
    transition: background-color 0.3s ease;
  }
  #admin-add-form button:hover {
    background: #4db0f7;
  }
  /* Responsive */
  @media (max-width: 768px) {
    #movie-list {
      grid-template-columns: repeat(auto-fill,minmax(160px,1fr));
    }
    .movie-poster {
      height: 220px;
    }
    #admin-panel {
      padding: 15px;
    }
  }
  /* Social Share */
  #social-share {
    margin-top: 20px;
  }
  #social-share button {
    background: #61dafb;
    border: none;
    border-radius: 6px;
    padding: 8px 14px;
    margin-right: 10px;
    cursor: pointer;
    color: #10121a;
    font-weight: 700;
    transition: background-color 0.3s ease;
  }
  #social-share button:hover {
    background: #4db0f7;
  }
</style>
</head>
<body>
<header>
  <h1>Nighters Movies</h1>
  <nav>
    <button id="user-view-btn" title="View Website as User">User View</button>
    <button id="admin-login-btn" title="Admin Login">Admin Login</button>
    <button id="logout-btn" title="Logout" class="hidden">Logout</button>
  </nav>
</header>
<main id="app">
  <!-- User Website -->
  <section id="user-website">
    <div id="auth-area">
      <h2 id="auth-title">Login</h2>
      <div id="auth-error" class="hidden"></div>
      <form id="auth-form">
        <label for="email">Email</label>
        <input type="email" id="email" placeholder="Enter email" required autocomplete="username"/>
        <label for="password">Password</label>
        <input type="password" id="password" placeholder="Enter password" required autocomplete="current-password" minlength="6"/>
        <button type="submit" id="auth-submit-btn">Login</button>
      </form>
      <p id="switch-to-register" style="text-align:center; margin-top: 12px; cursor: pointer; color:#61dafb;">Don't have an account? Register</p>
    </div>
    <div class="hidden" id="user-content-area">
      <div id="categories"></div>
      <div id="movie-list"></div>
      <div id="video-player-container" class="hidden">
        <video id="video-player" controls preload="metadata"></video>
        <div id="video-details"></div>
        <div id="social-share" class="hidden">
          <button id="share-facebook">Share Facebook</button>
          <button id="share-twitter">Share Twitter</button>
          <button id="share-whatsapp">Share WhatsApp</button>
        </div>
        <div id="reviews" class="hidden">
          <h3>User Reviews</h3>
          <div id="review-list"></div>
          <form id="add-review">
            <input type="text" id="review-text" placeholder="Write your review..." required minlength="3" />
            <select id="review-rating" required>
              <option value="" disabled selected>Rating</option>
              <option value="5">⭐⭐⭐⭐⭐</option>
              <option value="4">⭐⭐⭐⭐</option>
              <option value="3">⭐⭐⭐</option>
              <option value="2">⭐⭐</option>
              <option value="1">⭐</option>
            </select>
            <button type="submit" id="submit-review">Submit</button>
          </form>
        </div>
      </div>
    </div>
  </section>
  <!-- Admin Login -->
  <section id="admin-login-area" class="hidden" style="max-width: 400px; margin: 40px auto; background: #222832; padding: 24px 32px; border-radius: 12px; box-shadow: 0 2px 8px rgba(0,0,0,0.7); color: #e0e6f1;">
    <h2>Admin Panel Login</h2>
    <div id="admin-login-error" style="color: #ff7675; font-weight: 700; margin-bottom: 12px; display: none;"></div>
    <form id="admin-login-form">
      <label for="admin-password-input">Enter Admin Password</label>
      <input type="password" id="admin-password-input" placeholder="Password" autocomplete="current-password" required />
      <button type="submit" style="background: #61dafb; border: none; padding: 10px; width: 100%; border-radius: 6px; margin-top: 15px; color: #10121a; font-weight: 700; cursor: pointer;">Login</button>
    </form>
  </section>
  <!-- Admin Panel -->
  <section id="admin-panel" class="hidden" aria-label="Admin Panel">
    <h2>Administrator Panel</h2>
    <div id="content-management">
      <h3>Content Management</h3>
      <div id="admin-movie-list">
        <!-- Admin movies displayed here -->
      </div>
      <form id="admin-add-form" aria-label="Add or Edit Movie Content">
        <h4>Add / Edit Movie</h4>
        <input type="hidden" id="admin-movie-id" />
        <input type="text" id="admin-title" placeholder="Title" required />
        <textarea id="admin-description" placeholder="Description" rows="3" required></textarea>
        <select id="admin-category" required>
          <option value="" disabled selected>Select Category</option>
          <option value="Singer Movies">Singer Movies</option>
          <option value="Fantasy Series">Fantasy Series</option>
          <option value="Korean Series">Korean Series</option>
          <option value="Indian Series">Indian Series</option>
        </select>
        <input type="text" id="admin-poster" placeholder="Poster Image URL" required />
        <input type="text" id="admin-video-src" placeholder="Video Source URL (MP4/WebM/OGG)" required />
        <button type="submit">Add / Update Movie</button>
        <button type="button" id="admin-reset-btn" style="background:#ff5c5c; margin-top:8px; width: 100%;">Reset Form</button>
      </form>
    </div>
    <div id="user-management">
      <h3>User Management</h3>
      <div id="admin-user-list" style="max-height: 250px; overflow-y: auto; border-top: 1px solid #2a3450; border-bottom: 1px solid #2a3450; padding: 8px 0;">
        <!-- List of users with email -->
      </div>
    </div>
  </section>
</main>
<script>
(() => {
  "use strict";

  // Constants
  const ADMIN_PASSWORD = "i1s2a3a4c";

  // Elements
  const userWebsite = document.getElementById("user-website");
  const authArea = document.getElementById("auth-area");
  const authTitle = document.getElementById("auth-title");
  const authError = document.getElementById("auth-error");
  const authForm = document.getElementById("auth-form");
  const switchToRegister = document.getElementById("switch-to-register");

  const userContentArea = document.getElementById("user-content-area");
  const categoriesDiv = document.getElementById("categories");
  const movieListDiv = document.getElementById("movie-list");
  const videoPlayerContainer = document.getElementById("video-player-container");
  const videoPlayer = document.getElementById("video-player");
  const videoDetails = document.getElementById("video-details");
  const reviewsSection = document.getElementById("reviews");
  const reviewList = document.getElementById("review-list");
  const addReviewForm = document.getElementById("add-review");
  const reviewTextInput = document.getElementById("review-text");
  const reviewRatingSelect = document.getElementById("review-rating");

  const shareFacebookBtn = document.getElementById("share-facebook");
  const shareTwitterBtn = document.getElementById("share-twitter");
  const shareWhatsAppBtn = document.getElementById("share-whatsapp");
  const socialShareDiv = document.getElementById("social-share");

  const adminLoginArea = document.getElementById("admin-login-area");
  const adminLoginForm = document.getElementById("admin-login-form");
  const adminLoginError = document.getElementById("admin-login-error");
  const adminPasswordInput = document.getElementById("admin-password-input");

  const adminPanel = document.getElementById("admin-panel");
  const adminMovieList = document.getElementById("admin-movie-list");
  const adminUserList = document.getElementById("admin-user-list");
  const adminAddForm = document.getElementById("admin-add-form");
  const adminMovieIdInput = document.getElementById("admin-movie-id");
  const adminTitleInput = document.getElementById("admin-title");
  const adminDescriptionInput = document.getElementById("admin-description");
  const adminCategoryInput = document.getElementById("admin-category");
  const adminPosterInput = document.getElementById("admin-poster");
  const adminVideoSrcInput = document.getElementById("admin-video-src");
  const adminResetBtn = document.getElementById("admin-reset-btn");

  const userViewBtn = document.getElementById("user-view-btn");
  const adminLoginBtn = document.getElementById("admin-login-btn");
  const logoutBtn = document.getElementById("logout-btn");

  // Data in localStorage keys
  const LS_USERS_KEY = "nighters_users";
  const LS_CONTENT_KEY = "nighters_content";
  const LS_LOGGED_USER_KEY = "nighters_logged_user";
  const LS_ADMIN_LOGGED_KEY = "nighters_admin_logged";

  // State
  let state = {
    users: [],
    content: [],
    loggedUser: null,
    adminLogged: false,
    currentCategory: "All",
    playingMovie: null,
  };

  // Sample default content to bootstrap if none exists
  const defaultContent = [
    {
      id: "m1",
      title: "Legendary Singer Movie",
      description: "A biographical movie about the rise of a legendary singer.",
      category: "Singer Movies",
      poster:
        "https://images.unsplash.com/photo-1542206395-9feb3edaa68f?auto=format&fit=crop&w=400&q=80",
      videoSrc:
        "https://interactive-examples.mdn.mozilla.net/media/cc0-videos/flower.webm",
      ratings: [],
      reviews: [],
    },
    {
      id: "m2",
      title: "Magical Realms",
      description:
        "Fantasy series exploring enchanted worlds and brave heroes.",
      category: "Fantasy Series",
      poster:
        "https://images.unsplash.com/photo-1506744038136-46273834b3fb?auto=format&fit=crop&w=400&q=80",
      videoSrc:
        "https://interactive-examples.mdn.mozilla.net/media/cc0-videos/flower.webm",
      ratings: [],
      reviews: [],
    },
    {
      id: "m3",
      title: "Korean Drama Saga",
      description:
        "An epic tale of love and mystery in modern Korea.",
      category: "Korean Series",
      poster:
        "https://images.unsplash.com/photo-1504384308090-c894fdcc538d?auto=format&fit=crop&w=400&q=80",
      videoSrc:
        "https://interactive-examples.mdn.mozilla.net/media/cc0-videos/flower.webm",
      ratings: [],
      reviews: [],
    },
    {
      id: "m4",
      title: "Indian Family Series",
      description:
        "A heartwarming drama exploring family bonds and traditions.",
      category: "Indian Series",
      poster:
        "https://images.unsplash.com/photo-1542038784456-cb49e1727b56?auto=format&fit=crop&w=400&q=80",
      videoSrc:
        "https://interactive-examples.mdn.mozilla.net/media/cc0-videos/flower.webm",
      ratings: [],
      reviews: [],
    },
  ];

  // Categories including "All"
  const categories = [
    "All",
    "Singer Movies",
    "Fantasy Series",
    "Korean Series",
    "Indian Series",
  ];

  // Utility Functions
  function saveUsers() {
    localStorage.setItem(LS_USERS_KEY, JSON.stringify(state.users));
  }
  function loadUsers() {
    let saved = localStorage.getItem(LS_USERS_KEY);
    if (saved) {
      try {
        state.users = JSON.parse(saved);
      } catch {
        state.users = [];
      }
    } else {
      state.users = [];
    }
  }

  function saveContent() {
    localStorage.setItem(LS_CONTENT_KEY, JSON.stringify(state.content));
  }
  function loadContent() {
    let saved = localStorage.getItem(LS_CONTENT_KEY);
    if (saved) {
      try {
        state.content = JSON.parse(saved);
      } catch {
        state.content = [];
      }
    } else {
      state.content = [];
    }
  }

  function saveLoggedUser() {
    if (state.loggedUser) {
      localStorage.setItem(LS_LOGGED_USER_KEY, JSON.stringify(state.loggedUser));
    } else {
      localStorage.removeItem(LS_LOGGED_USER_KEY);
    }
  }
  function loadLoggedUser() {
    let saved = localStorage.getItem(LS_LOGGED_USER_KEY);
    if (saved) {
      try {
        state.loggedUser = JSON.parse(saved);
      } catch {
        state.loggedUser = null;
      }
    } else {
      state.loggedUser = null;
    }
  }

  function saveAdminLogged() {
    localStorage.setItem(LS_ADMIN_LOGGED_KEY, state.adminLogged ? "true" : "false");
  }
  function loadAdminLogged() {
    const saved = localStorage.getItem(LS_ADMIN_LOGGED_KEY);
    state.adminLogged = saved === "true";
  }

  // Simple user password hashing for demo (don't use in production)
  function hashPassword(password) {
    let hash = 0,
      i,
      chr;
    if (password.length === 0) return hash.toString();
    for (i = 0; i < password.length; i++) {
      chr = password.charCodeAt(i);
      hash = (hash << 5) - hash + chr;
      hash |= 0; // Convert to 32bit integer
    }
    return hash.toString();
  }

  // Authentication
  function registerUser(email, password) {
    if (state.users.find((u) => u.email === email.toLowerCase())) {
      return { success: false, message: "User with this email already exists." };
    }
    const user = {
      id: crypto.randomUUID(),
      email: email.toLowerCase(),
      passwordHash: hashPassword(password),
      ratings: {},
      reviews: {},
    };
    state.users.push(user);
    saveUsers();
    return { success: true, user };
  }

  function loginUser(email, password) {
    const user = state.users.find(
      (u) => u.email === email.toLowerCase() && u.passwordHash === hashPassword(password)
    );
    if (!user) {
      return { success: false, message: "Invalid email or password." };
    }
    state.loggedUser = user;
    saveLoggedUser();
    return { success: true, user };
  }

  function logoutUser() {
    state.loggedUser = null;
    saveLoggedUser();
  }

  // UI Functions
  function setAuthError(message) {
    authError.textContent = message;
    if (message) authError.classList.remove("hidden");
    else authError.classList.add("hidden");
  }

  // Switch login <-> register UI
  let isLoginMode = true;
  function toggleAuthMode() {
    isLoginMode = !isLoginMode;
    if (isLoginMode) {
      authTitle.textContent = "Login";
      authSubmitBtn.textContent = "Login";
      switchToRegister.textContent = "Don't have an account? Register";
    } else {
      authTitle.textContent = "Register";
      authSubmitBtn.textContent = "Register";
      switchToRegister.textContent = "Already have an account? Login";
    }
    setAuthError("");
  }

  // Display categories
  function renderCategories() {
    categoriesDiv.innerHTML = "";
    categories.forEach((cat) => {
      const btn = document.createElement("button");
      btn.textContent = cat;
      btn.disabled = false;
      if (cat === state.currentCategory) {
        btn.classList.add("active");
      }
      btn.addEventListener("click", () => {
        state.currentCategory = cat;
        renderCategories();
        renderMovieList();
        hideVideoPlayer();
      });
      categoriesDiv.appendChild(btn);
    });
  }

  // Display movie list by category
  function renderMovieList() {
    movieListDiv.innerHTML = "";
    let filtered = state.content;
    if (state.currentCategory !== "All") {
      filtered = filtered.filter((item) => item.category === state.currentCategory);
    }
    if (filtered.length === 0) {
      movieListDiv.innerHTML = `<p>No content available in this category.</p>`;
      return;
    }
    filtered.forEach((movie) => {
      const card = document.createElement("article");
      card.classList.add("movie-card");
      card.setAttribute("tabindex", 0);
      card.setAttribute("role", "button");
      card.setAttribute("aria-label", `Watch ${movie.title}`);

      card.innerHTML = `
        <img class="movie-poster" src="${movie.poster}" alt="Poster of ${escapeHtml(movie.title)}"/>
        <div class="movie-content">
          <h3 class="movie-title">${escapeHtml(movie.title)}</h3>
          <p class="movie-desc">${escapeHtml(movie.description)}</p>
          <p class="movie-genre">${escapeHtml(movie.category)}</p>
          <div class="movie-rating" aria-label="Average rating">${renderStars(computeAverageRating(movie.ratings))}</div>
        </div>
      `;
      card.addEventListener("click", () => {
        playMovie(movie.id);
      });
      card.addEventListener("keydown", ev => {
        if (ev.key === "Enter" || ev.key === " ") {
          ev.preventDefault();
          playMovie(movie.id);
        }
      });
      movieListDiv.appendChild(card);
    });
  }

  // Escape HTML to prevent injection in text
  function escapeHtml(text) {
    if (!text) return "";
    return text.replace(/[&<>"']/g, (m) => {
      switch (m) {
        case "&":
          return "&amp;";
        case "<":
          return "&lt;";
        case ">":
          return "&gt;";
        case '"':
          return "&quot;";
        case "'":
          return "&#039;";
        default:
          return m;
      }
    });
  }

  // Compute average rating from array of numbers
  function computeAverageRating(ratings) {
    if (!ratings?.length) return 0;
    let sum = ratings.reduce((a, b) => a + b, 0);
    return sum / ratings.length;
  }

  // Render stars for a rating (average rating) up to 5
  function renderStars(avgRating) {
    const fullStars = Math.floor(avgRating);
    const halfStar = avgRating - fullStars >= 0.5;
    let starsHtml = "";
    for (let i = 1; i <= 5; i++) {
      if (i <= fullStars) starsHtml += '<span class="star">★</span>';
      else if (i === fullStars + 1 && halfStar) starsHtml += '<span class="star">☆</span>';
      else starsHtml += '<span class="star" style="color: #666;">☆</span>';
    }
    return starsHtml;
  }

  // Play movie by id
  function playMovie(movieId) {
    const movie = state.content.find((m) => m.id === movieId);
    if (!movie) return;
    state.playingMovie = movie;
    videoPlayerContainer.classList.remove("hidden");
    videoPlayer.src = movie.videoSrc;
    videoPlayer.setAttribute("aria-label", `Video player for ${movie.title}`);
    videoPlayer.load();
    videoPlayer.play().catch(() => {});
    videoDetails.textContent = `${movie.title} | Category: ${movie.category}`;
    renderReviews(movie);
    socialShareDiv.classList.remove("hidden");
    reviewsSection.classList.remove("hidden");
  }

  function hideVideoPlayer() {
    videoPlayer.pause();
    videoPlayer.src = "";
    videoDetails.textContent = "";
    videoPlayerContainer.classList.add("hidden");
    reviewsSection.classList.add("hidden");
    socialShareDiv.classList.add("hidden");
    state.playingMovie = null;
  }

  // Render reviews for movie
  function renderReviews(movie) {
    reviewList.innerHTML = "";
    if (!movie.reviews || movie.reviews.length === 0) {
      reviewList.textContent = "No reviews yet. Be the first to write one!";
      return;
    }
    movie.reviews.forEach((r) => {
      const div = document.createElement("div");
      div.className = "review-item";
      div.innerHTML = `
        <div class="review-user">${escapeHtml(r.userEmail)}</div>
        <div class="review-rating">${renderStars(r.rating)}</div>
        <div class="review-text">${escapeHtml(r.text)}</div>
      `;
      reviewList.appendChild(div);
    });
  }

  // Add review handler
  addReviewForm.addEventListener("submit", (e) => {
    e.preventDefault();
    if (!state.loggedUser) return alert("You must be logged in to leave a review.");
    if (!state.playingMovie) return;

    const text = reviewTextInput.value.trim();
    const rating = Number(reviewRatingSelect.value);
    if (text.length < 3) {
      alert("Review text must be at least 3 characters.");
      return;
    }
    if (!(rating >= 1 && rating <= 5)) {
      alert("Please select a valid rating.");
      return;
    }
    // Add review & rating to movie
    const movieIndex = state.content.findIndex((m) => m.id === state.playingMovie.id);
    if (movieIndex === -1) return;

    const newReview = {
      userEmail: state.loggedUser.email,
      text,
      rating,
    };
    state.content[movieIndex].reviews.push(newReview);
    state.content[movieIndex].ratings.push(rating);
    saveContent();
    renderReviews(state.content[movieIndex]);
    renderMovieList();
    reviewTextInput.value = "";
    reviewRatingSelect.value = "";
  });

  // Auth form submission handler (login/register)
  authForm.addEventListener("submit", (e) => {
    e.preventDefault();

    const email = authForm.email.value.trim();
    const password = authForm.password.value;

    if (!validateEmail(email)) {
      setAuthError("Please enter a valid email address.");
      return;
    }
    if(password.length < 6) {
      setAuthError("Password must be at least 6 characters.");
      return;
    }

    if (isLoginMode) {
      const loginResult = loginUser(email, password);
      if (loginResult.success) {
        setAuthError("");
        showUserContent();
      } else {
        setAuthError(loginResult.message);
      }
    } else {
      const regResult = registerUser(email, password);
      if (regResult.success) {
        setAuthError("");
        alert("Registration successful. You can now log in.");
        toggleAuthMode();
      } else {
        setAuthError(regResult.message);
      }
    }
  });

  // Toggle login/register
  switchToRegister.addEventListener("click", () => {
    toggleAuthMode();
  });

  function validateEmail(email) {
    // Simple regex for email validation
    const re = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    return re.test(email.toLowerCase());
  }

  // Show user content after login
  function showUserContent() {
    authArea.classList.add("hidden");
    userContentArea.classList.remove("hidden");
    logoutBtn.classList.remove("hidden");
    userViewBtn.disabled = true;
    adminLoginBtn.disabled = true;
    renderCategories();
    renderMovieList();
  }

  // Show login/register form
  function showAuthForm() {
    authArea.classList.remove("hidden");
    userContentArea.classList.add("hidden");
    hideVideoPlayer();
    logoutBtn.classList.add("hidden");
    userViewBtn.disabled = false;
    adminLoginBtn.disabled = false;
    setAuthError("");
    authForm.email.value = "";
    authForm.password.value = "";
  }

  // Logout user or admin
  logoutBtn.addEventListener("click", () => {
    if (state.adminLogged) {
      logoutAdmin();
    } else if (state.loggedUser) {
      logoutUser();
      showAuthForm();
    }
    logoutBtn.classList.add("hidden");
  });

  // User View button forces user view mode (logout admin)
  userViewBtn.addEventListener("click", () => {
    if (state.adminLogged) {
      logoutAdmin();
    }
    // Reset to user login screen (if no user logged, show login)
    if (state.loggedUser) {
      showUserContent();
    } else {
      showAuthForm();
    }
  });

  // Admin Login button shows admin login area
  adminLoginBtn.addEventListener("click", () => {
    if(state.adminLogged) {
      // Already logged in, switch to admin panel view
      showAdminPanel();
      return;
    }
    // Show admin login form
    userWebsite.classList.add("hidden");
    adminLoginArea.classList.remove("hidden");
    adminLoginError.style.display = "none";
    adminPasswordInput.value = "";
    logoutBtn.classList.add("hidden");
    userViewBtn.disabled = true;
    adminLoginBtn.disabled = true;
  });

  // Admin login form submission
  adminLoginForm.addEventListener("submit", (e) => {
    e.preventDefault();
    const pwd = adminPasswordInput.value.trim();
    if(pwd === ADMIN_PASSWORD) {
      state.adminLogged = true;
      saveAdminLogged();
      adminLoginArea.classList.add("hidden");
      showAdminPanel();
      logoutBtn.classList.remove("hidden");
      userViewBtn.disabled = true;
      adminLoginBtn.disabled = true;
    } else {
      adminLoginError.textContent = "Incorrect admin password.";
      adminLoginError.style.display = "block";
    }
  });

  function logoutAdmin() {
    state.adminLogged = false;
    saveAdminLogged();
    adminPanel.classList.add("hidden");
    userWebsite.classList.remove("hidden");
    showAuthForm();
    logoutBtn.classList.add("hidden");
    userViewBtn.disabled = false;
    adminLoginBtn.disabled = false;
  }

  // Show admin panel
  function showAdminPanel() {
    adminPanel.classList.remove("hidden");
    userWebsite.classList.add("hidden");
    adminLoginArea.classList.add("hidden");
    renderAdminMovieList();
    renderAdminUserList();
    clearAdminForm();
  }

  // Admin movies list rendering
  function renderAdminMovieList() {
    adminMovieList.innerHTML = "";
    if(state.content.length === 0) {
      adminMovieList.innerHTML = "<p>No movies content.</p>";
      return;
    }
    state.content.forEach((movie) => {
      const div = document.createElement("div");
      div.className = "admin-movie-item";
      div.innerHTML = `
        <strong>${escapeHtml(movie.title)}</strong>
        <div class="admin-buttons">
          <button class="admin-button btn-edit" title="Edit Movie">✏️</button>
          <button class="admin-button btn-delete" title="Delete Movie" style="background:#ff5c5c;">🗑️</button>
        </div>
      `;
      // Edit button
      div.querySelector(".btn-edit").addEventListener("click", () => {
        adminMovieIdInput.value = movie.id;
        adminTitleInput.value = movie.title;
        adminDescriptionInput.value = movie.description;
        adminCategoryInput.value = movie.category;
        adminPosterInput.value = movie.poster;
        adminVideoSrcInput.value = movie.videoSrc;
        window.scrollTo({ top: adminPanel.offsetTop, behavior: 'smooth' });
      });
      // Delete button
      div.querySelector(".btn-delete").addEventListener("click", () => {
        if(confirm(`Delete movie "${movie.title}"? This action cannot be undone.`)) {
          state.content = state.content.filter((m) => m.id !== movie.id);
          saveContent();
          renderAdminMovieList();
          renderCategories();
          renderMovieList();
          if(state.playingMovie && state.playingMovie.id === movie.id) {
            hideVideoPlayer();
          }
        }
      });
      adminMovieList.appendChild(div);
    });
  }

  // Clear admin form
  function clearAdminForm() {
    adminMovieIdInput.value = "";
    adminTitleInput.value = "";
    adminDescriptionInput.value = "";
    adminCategoryInput.value = "";
    adminPosterInput.value = "";
    adminVideoSrcInput.value = "";
  }
  adminResetBtn.addEventListener("click", () => {
    clearAdminForm();
  });

  // Admin form submission: add or edit movie
  adminAddForm.addEventListener("submit", (e) => {
    e.preventDefault();
    // Validation
    const title = adminTitleInput.value.trim();
    const description = adminDescriptionInput.value.trim();
    const category = adminCategoryInput.value;
    const poster = adminPosterInput.value.trim();
    const videoSrc = adminVideoSrcInput.value.trim();

    if(!title || !description || !category || !poster || !videoSrc) {
      alert("All fields are required.");
      return;
    }
    const id = adminMovieIdInput.value;

    if(id) {
      // Edit existing
      const index = state.content.findIndex((m) => m.id === id);
      if(index !== -1) {
        state.content[index].title = title;
        state.content[index].description = description;
        state.content[index].category = category;
        state.content[index].poster = poster;
        state.content[index].videoSrc = videoSrc;
      }
    } else {
      // New movie
      state.content.push({
        id: crypto.randomUUID(),
        title,
        description,
        category,
        poster,
        videoSrc,
        ratings: [],
        reviews: [],
      });
    }
    saveContent();
    renderAdminMovieList();
    renderCategories();
    renderMovieList();
    clearAdminForm();
    alert("Movie content saved successfully.");
  });

  // Admin user list rendering
  function renderAdminUserList() {
    adminUserList.innerHTML = "";
    if(state.users.length === 0) {
      adminUserList.innerHTML = "<p>No registered users.</p>";
      return;
    }
    state.users.forEach((user) => {
      const div = document.createElement("div");
      div.style.padding = "6px 8px";
      div.style.borderBottom = "1px solid #2a3450";
      div.textContent = user.email;
      adminUserList.appendChild(div);
    });
  }

  // Social sharing handlers
  function constructShareURL(platform, url, title) {
    switch(platform) {
      case 'facebook':
        return `https://www.facebook.com/sharer/sharer.php?u=${encodeURIComponent(url)}`;
      case 'twitter':
        return `https://twitter.com/intent/tweet?url=${encodeURIComponent(url)}&text=${encodeURIComponent(title)}`;
      case 'whatsapp':
        return `https://api.whatsapp.com/send?text=${encodeURIComponent(title + " - " + url)}`;
      default:
        return '#';
    }
  }

  function share(platform) {
    if (!state.playingMovie) return;
    const url = window.location.href.split('#')[0];
    const shareUrl = constructShareURL(platform, url, state.playingMovie.title);
    window.open(shareUrl, '_blank', 'noopener');
  }

  shareFacebookBtn.addEventListener("click", () => share("facebook"));
  shareTwitterBtn.addEventListener("click", () => share("twitter"));
  shareWhatsAppBtn.addEventListener("click", () => share("whatsapp"));

  // Initial load
  function initialize() {
    loadUsers();
    loadContent();
    if(state.content.length === 0) {
      state.content = defaultContent;
      saveContent();
    }
    loadLoggedUser();
    loadAdminLogged();

    // If admin is logged, show admin panel
    if (state.adminLogged) {
      showAdminPanel();
      logoutBtn.classList.remove("hidden");
    }
    // else if user is logged, show user content area
    else if (state.loggedUser) {
      showUserContent();
    } else {
      showAuthForm();
    }
  }

  // Run initialization
  initialize();
})();
</script>
</body>
</html>

© 2025 Nigters Movies . All Rights Reserved.
